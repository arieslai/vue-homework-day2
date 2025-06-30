<script setup>
import { onMounted, ref } from 'vue';

const newTitle = ref('')
const newUrl = ref('')
const urlCollections = ref([])
const auoUrlCollectionStorageKey = 'auo-url-collection-tool'
const urlError = ref(false)

onMounted(() => {
  const storedUrlCollections = localStorage.getItem(auoUrlCollectionStorageKey);
  if (storedUrlCollections) {
    urlCollections.value = JSON.parse(storedUrlCollections);
  }
})

const addNewUrlCollection = () => {
  if (newUrl.value != '') {
    urlError.value = false
    const urlCollection = {
      id: crypto.randomUUID(),
      url: newUrl.value,
      title: newTitle.value
    }
    urlCollections.value.unshift(urlCollection)
    localStorage.setItem(auoUrlCollectionStorageKey, JSON.stringify(urlCollections.value))
    clearInput()
  }
  else {
    urlError.value = true
  }
}

const clearInput = () => {
  newTitle.value = ''
  newUrl.value = ''
  urlError.value = false
}

const deleteAll = () => {
  if (confirm(`確定清空全部`)) {
    urlCollections.value = []
    setLocalStorage()
  }
}

const deleteUrlCollection = (urlCollection) => {
  if (confirm(`確定刪除：${formatUrlCollection(urlCollection)}`)) {
    urlCollections.value = urlCollections.value.filter(e => e.id !== urlCollection.id)
    setLocalStorage()
  }
}

const setLocalStorage = () => {
  localStorage.setItem(auoUrlCollectionStorageKey, JSON.stringify(urlCollections.value))
}

const formatUrlCollection = (urlCollection) => {
  return `${urlCollection.title != '' ? `[${urlCollection.title}] ` : ''}${urlCollection.url}`
}
</script>

<template>
  <div data-theme="dark" class="min-h-screen bg-base-200 p-6">
    <div class="max-w-4xl mx-auto">
      <!-- 標題區域 -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-primary-content mb-2">收藏網址小工具</h1>
        <div class="w-24 h-1 bg-gradient-to-r from-primary to-secondary mx-auto rounded-full"></div>
      </div>

      <!-- 輸入區域 -->
      <div class="card bg-base-100 shadow-xl mb-8">
        <div class="card-body">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
            <div class="form-control">
              <label class="label mr-4">
                <span class="label-text text-base-content/80">標題</span>
              </label>
              <input @keyup.enter="addNewUrlCollection" v-model.trim="newTitle" type="text" placeholder="輸入標題..."
                class="input input-bordered input-primary focus:input-primary" />
            </div>
            <div class="form-control">
              <label class="label mr-4">
                <span class="label-text text-base-content/80">網址 <span class="text-error">*</span></span>
              </label>
              <input @keyup.enter="addNewUrlCollection" v-model.trim="newUrl" type="text" placeholder="輸入網址..."
                :class="['input input-bordered input-primary focus:input-primary', urlError ? 'input-error' : '']" />
              <p v-if="urlError" class="mt-1 text-error text-sm">網址為必填欄位</p>
            </div>
          </div>

          <div class="flex flex-col sm:flex-row gap-3 justify-center">
            <button @click="addNewUrlCollection" class="btn btn-primary btn-lg flex-1 sm:flex-none">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
              </svg>
              新增收藏
            </button>
            <button @click="deleteAll" class="btn btn-error btn-outline btn-lg flex-1 sm:flex-none">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
              清除全部
            </button>
          </div>
        </div>
      </div>

      <!-- 收藏列表區域 -->
      <div class="card bg-base-100 shadow-xl" v-if="urlCollections.length > 0">
        <div class="card-body">
          <h2 class="card-title text-2xl mb-6 text-primary-content">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24"
              stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M5 5a2 2 0 012-2h10a2 2 0 012 2v16l-7-3.5L5 21V5z" />
            </svg>
            我的收藏 ({{ urlCollections.length }})
          </h2>

          <div class="space-y-3">
            <div v-for="urlCollection in urlCollections" :key="urlCollection.id"
              class="flex items-center justify-between p-4 bg-base-200 rounded-lg border border-base-300 hover:border-primary transition-all duration-200 hover:shadow-md">
              <div class="flex-1 min-w-0 mr-4">
                <a :href="urlCollection.url" target="_blank"
                  class="text-primary-content hover:text-primary-content-focus font-medium break-all">
                  {{ formatUrlCollection(urlCollection) }}
                </a>
              </div>

              <button @click="deleteUrlCollection(urlCollection)"
                class="btn btn-error btn-sm btn-outline hover:btn-error" title="刪除此收藏">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24"
                  stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- 空狀態 -->
      <div v-else class="text-center py-16">
        <div class="text-6xl mb-4">📚</div>
        <h3 class="text-2xl font-semibold text-base-content/60 mb-2">尚無收藏</h3>
        <p class="text-base-content/40">開始新增你的第一個收藏網址吧！</p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

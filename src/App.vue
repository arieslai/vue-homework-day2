<script setup>
import { onMounted, ref } from 'vue';

const newTitle = ref('')
const newUrl = ref('')
const urlCollections = ref([])
const auoUrlCollectionStorageKey = 'auo-url-collection-tool'

onMounted(() => {
  const storedUrlCollections = localStorage.getItem(auoUrlCollectionStorageKey);
  if (storedUrlCollections) {
    urlCollections.value = JSON.parse(storedUrlCollections);
  }
})

const addNewUrlCollection = () => {
  if (newUrl.value != '') {
    const urlCollection = {
      id: crypto.randomUUID(),
      url: newUrl.value,
      title: newTitle.value
    }
    urlCollections.value.unshift(urlCollection)
    localStorage.setItem(auoUrlCollectionStorageKey, JSON.stringify(urlCollections.value))
    clearInput()
  }
  else
    alert('請填寫網址')

}

const clearInput = () => {
  newTitle = ''
  newUrl = ''
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
  localStorage.setItem(auoUrlCollectionStorageKey, urlCollections.value)
}

const formatUrlCollection = (urlCollection) => {
  return `${urlCollection.titel != '' ? `[${urlCollection.title}]` : ''}(${urlCollection.url})`
}
</script>

<template>
  <div>
    <h1>收藏網址⼩⼯具</h1>
    <input @keyup.enter="addNewUrlCollection" v-model.trim="newTitle" type="text" class="input" placeholder="標題" />
    <input @keyup.enter="addNewUrlCollection" v-model.trim="newUrl" type="text" class="input" placeholder="網址" />
    <button @click="addNewUrlCollection">新增收藏</button>
    <button @click="deleteAll">清除全部</button>
    <ul v-for="urlCollection in urlCollections">
      <li>{{ formatUrlCollection(urlCollection) }}</li><button @click="deleteUrlCollection(urlCollection)">刪除收藏</button>
    </ul>
  </div>
</template>

<style scoped></style>

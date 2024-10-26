<script setup>
import axios from 'axios'
import CardList from '../components/CardList.vue'

import { inject, reactive, watch, onMounted, ref } from 'vue'

const items = ref([])

const { addToDrawer, removeFromDrawer, drawerItems } = inject('drawer')

const onClickAddPlus = item => {
  if (!item.isAdded) {
    addToDrawer(item)
  } else {
    removeFromDrawer(item)
  }
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const onChangeSelect = event => {
  filters.sortBy = event.target.value
}
const onChangeInput = event => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      'https://32a9ed52bd94d242.mokky.dev/favorites',
    )

    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const fetchData = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(
      'https://32a9ed52bd94d242.mokky.dev/items',
      { params },
    )
    items.value = data.map(obj => ({
      ...obj,
      isAdded: false,
      favoriteId: null,
      isFavorite: false,
    }))
  } catch (error) {
    console.log(error)
  }
}

const addToFavorite = async item => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
      }
      item.isFavorite = true

      const { data } = await axios.post(
        'https://32a9ed52bd94d242.mokky.dev/favorites',
        obj,
      )
      item.favoriteId = data.Id
    } else {
      item.isFavorite = false
      await axios.delete(
        `https://32a9ed52bd94d242.mokky.dev/favorites/${item.favoriteId}`,
      )
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localDrawer = localStorage.getItem('drawerItems')
  drawerItems.value = localDrawer ? JSON.parse(localDrawer) : []

  await fetchData()
  await fetchFavorites()

  items.value = items.value.map(item => ({
    ...item,
    isAdded: drawerItems.value.some(drawerItem => drawerItem.id === item.id),
  }))
})

watch(drawerItems, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: false,
  }))
})

watch(filters, fetchData)
</script>

<template>
  <div class="flex justify-between mb-10">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>

    <div class="flex gap-10">
      <select
        @change="onChangeSelect"
        class="py-2 px-3 border rounded-md outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img class="absolute top-3 left-3" src="/search.svg" alt="Search" />
        <input
          @input="onChangeInput"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          placeholder="Поиск..."
          type="text"
        />
      </div>
    </div>
  </div>

  <CardList
    :items="items"
    @add-to-favorite="addToFavorite"
    @add-to-drawer="onClickAddPlus"
  />
</template>

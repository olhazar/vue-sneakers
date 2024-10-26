<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://32a9ed52bd94d242.mokky.dev/favorites?_relations=items',
    )

    favorites.value = data.map(obj => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <CardList :items="favorites" is-favorites />

  <div class="flex flex-col items-center" v-if="!favorites.length">
    <img
      class="mt-16 mb-4"
      width="70"
      height="70"
      src="/not-favorites.jpg"
      alt="not-favorites"
    />
    <h2 class="mb-2 text-2xl font-bold">Закладок нет :(</h2>
    <p class="text-slate-500">Вы ничего не добавляли в закладки</p>
  </div>
</template>

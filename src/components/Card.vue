<script setup>
const props = defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      v-if="onClickFavorite"
      :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
      alt="Like 2"
      class="absolute top-8 left-8"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="Sneaker" />

    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-4">
      <div class="flex flex-col">
        <span class="text-slate-500">Цена:</span>
        <b>{{ price }} ₽</b>
      </div>

      <img
        v-if="onClickAdd"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        alt="Plus"
        @click="onClickAdd"
      />
    </div>
  </div>
</template>

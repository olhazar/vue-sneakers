<script setup>
import { inject } from 'vue'

const { closeDrawer } = inject('drawer')
</script>

<template>
  <div class="flex flex-col items-center text-center">
    <img height="100" width="100" src="/package-icon.png" alt="Empty" />
    <h2 class="mt-4 text-2xl font-medium">Корзина пустая</h2>
    <p class="mt-2">
      Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ.
    </p>
    <button
      @click="closeDrawer"
      class="mt-4 transition w-full bg-lime-500 rounded-xl text-white p-4 disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
    >
      Вернуться назад
    </button>
  </div>
</template>

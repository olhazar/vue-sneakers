<script setup>
import { inject } from 'vue'

defineProps({
  totalPrice: Number,
})

const { openDrawer } = inject('drawer')
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-8 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-500">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-8">
      <li
        @click="openDrawer"
        class="flex items-center gap-2 cursor-pointer text-slate-500 hover:text-black transition ease-out"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} {{ totalPrice > 0 ? '₽' : null }}</b>
      </li>

      <router-link to="/favorites">
        <li
          class="flex items-center gap-2 cursor-pointer text-slate-500 hover:text-black transition ease-out"
        >
          <img src="/heart.svg" alt="Heart" />
          <span>Закладки</span>
        </li></router-link
      >

      <router-link to="/profile">
        <li
          class="flex items-center gap-2 cursor-pointer text-slate-500 hover:text-black transition ease-out"
        >
          <img src="/profile.svg" alt="Profile" />
          <span>Профиль</span>
        </li></router-link
      >
    </ul>
  </header>
</template>

<script setup>
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

import Home from './pages/Home.vue'

import { ref, watch, provide, computed } from 'vue'

import axios from 'axios'

// Корзина //

const drawerItems = ref([])
const isDrawerOpen = ref(false)

const totalPrice = computed(() =>
  drawerItems.value.reduce((acc, item) => acc + item.price, 0),
)
const taxPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const openDrawer = () => {
  isDrawerOpen.value = true
}
const closeDrawer = () => {
  isDrawerOpen.value = false
}

const addToDrawer = item => {
  drawerItems.value.push(item)
  item.isAdded = true
}

const removeFromDrawer = item => {
  drawerItems.value.splice(drawerItems.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  drawerItems,
  () => {
    localStorage.setItem('drawerItems', JSON.stringify(drawerItems.value))
  },
  { deep: true },
)

provide('drawer', {
  drawerItems,
  openDrawer,
  closeDrawer,
  addToDrawer,
  removeFromDrawer,
})

// Корзина //
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Drawer
      v-if="isDrawerOpen"
      :total-price="totalPrice"
      :tax-price="taxPrice"
    />

    <Header :totalPrice="totalPrice" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>

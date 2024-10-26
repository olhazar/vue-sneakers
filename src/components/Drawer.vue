<script setup>
import axios from 'axios'

const props = defineProps({
  totalPrice: Number,
  taxPrice: Number,
})

import { inject, ref } from 'vue'
import DrawerItemList from './DrawerItemList.vue'
import DrawerEmpty from './DrawerEmpty.vue'
import Order from './Order.vue'

const { drawerItems, closeDrawer } = inject('drawer')

const orderId = ref(null)

const createOrder = async () => {
  try {
    const { data } = await axios.post(
      'https://32a9ed52bd94d242.mokky.dev/orders',
      {
        items: drawerItems.value,
        totalPrice: props.totalPrice.value,
      },
    )

    drawerItems.value = []
    orderId.value = data.id
  } catch (err) {
    console.log(err)
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black opacity-70 z-10"></div>

  <div class="bg-white w-96 fixed right-0 top-0 z-20 h-full p-8">
    <div class="flex items-center gap-5 mb-6">
      <img
        @click="closeDrawer"
        class="opacity-50 cursor-pointer rotate-180 transition hover:opacity-100 hover:-translate-x-1"
        src="/arrow-next.svg"
        alt="back"
      />
      <h2 class="text-2xl font-bold">Корзина</h2>
    </div>

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <DrawerEmpty v-if="!totalPrice && !orderId" />
      <Order v-if="orderId" :order-id="orderId" />
    </div>

    <DrawerItemList />

    <div v-if="totalPrice" class="flex flex-col gap-4 mt-7">
      <div class="flex">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} ₽</b>
      </div>

      <div class="flex">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ taxPrice }} ₽</b>
      </div>

      <button
        @click="createOrder"
        :disabled="totalPrice ? false : true"
        class="mt-4 transition w-full bg-lime-500 rounded-xl text-white p-4 disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>

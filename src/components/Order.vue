<script setup>
defineProps({
  orderId: Number,
})
</script>

<template>
  <div class="flex flex-col items-center text-center">
    <img height="70" width="70" src="/order-success-icon.png" alt="Order" />
    <h2 class="mt-4 text-2xl font-medium">Заказ оформлен!</h2>
    <p class="mt-2">Ваш заказ #{{ orderId }} был успешно оформлен</p>
  </div>
</template>

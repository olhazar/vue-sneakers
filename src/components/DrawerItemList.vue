<script setup>
import DrawerItem from './DrawerItem.vue'
import { inject } from 'vue'

const { drawerItems, removeFromDrawer } = inject('drawer')
</script>

<template>
  <div class="flex flex-col gap-4 scrollbar" v-auto-animate>
    <DrawerItem
      v-for="item in drawerItems"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromDrawer(item)"
    />
  </div>
</template>

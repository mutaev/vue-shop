<script setup>
import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'

import Header from '@/components/Header.vue'
import Drawer from '@/components/Drawer.vue'
import HomeView from '@/pages/HomeView.vue'

// корзина start
const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {
    deep: true
  }
)

provide('cart', {
  closeDrawer,
  openDrawer,
  cart,
  addToCart,
  removeFromCart
})
// корзина end
</script>

<template>
  <Drawer :total-price="totalPrice" :vat-price="vatPrice" v-if="drawerOpen" />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

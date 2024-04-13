<script setup>
import CartItem from '@/components/CartItem.vue'
import { inject } from 'vue'

const { cart, removeFromCart } = inject('cart')

</script>

<template>
  <div class="flex flex-col flex-1 gap-4">
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :price="item.price"
      :image-url="item.imageUrl"
      :title="item.title"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>


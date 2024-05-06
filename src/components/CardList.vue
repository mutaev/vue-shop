<script setup>
import Card from '@/components/Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :on-click-favorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :on-click-add="isFavorites ? null : () => emit('addToCart', item)"
      :isAdded="item.isAdded"
      :is-favorite="item.isFavorite"
    />
  </div>
</template>

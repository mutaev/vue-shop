<script setup lang="ts">
import Card from '@/components/Card.vue'

const onClickAdd = () => {
  alert("Добавить!")
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card title="Мужчинские кроссовки брат"
          imageUrl="/sneakers/sneakers-1.jpg"
          :price="5000"
          :is-added="true"
          :on-click-add="onClickAdd"
    />
  </div>
</template>


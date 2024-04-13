<script setup>
  import { inject } from 'vue'

  const emit = defineEmits(['onClickRemove'])

  defineProps({
    title: String,
    id: Number,
    imageUrl: String,
    price: Number,
  })

</script>

<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" alt="Sneaker" />

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} руб.</b>
        <img @click="emit('onClickRemove')" class="opacity-40 hover:opacity-100 transition cursor-pointer" src="/close.svg">
      </div>
    </div>
  </div>
</template>



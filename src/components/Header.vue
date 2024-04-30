<script setup>
  const emit = defineEmits(['openDrawer'])

  defineProps({
    totalPrice: Number
    })
</script>

<template>
  <header class="flex justify-between border-b border-slate-100 px-10 py-8" >
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img class="w-10" src="../assets/logo.png" alt="logo">
        <div>
          <h2 class="text-xl font-bold uppercase">Mutaev Shop</h2>
          <p class="text-slate-400">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li @click="() => emit('openDrawer')" class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
        <img src="/cart.svg" alt="Cart">
        <b>{{ totalPrice }} руб.</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
          <img src="/heart.svg" alt="Cart">
          <span>Закладки</span>
        </li>
      </router-link>
      <li class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
        <img src="/profile.svg" alt="Cart">
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>

<style scoped>

</style>

<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartListItem from '@/components/CartListItem.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-60"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col flex-1">
    <DrawerHead />

    <CartListItem/>

    <div class="flex flex-col gap-4 my-7 ">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>12900 Руб.</b>
      </div>

      <div class="flex gap-2">
        <span>Налог 5%</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>900 Руб.</b>
      </div>
      <button
        disabled=""
        class=" mt-3 cursor-pointer disabled:bg-slate-300 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700">
        Офорить заказ
      </button>
    </div>

  </div>
</template>
<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartListItem from '@/components/CartListItem.vue'
import { computed, inject, ref } from 'vue'
import InfoBlock from '@/components/InfoBlock.vue'
import axios from 'axios'
const emit = defineEmits(['createOrder'])

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { cart, closeDrawer } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(`https://8b0c688acbe64a0b.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-60"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col flex-1">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        description="Ваш заказ скоро будет передан курьерской доставке"
        image-url="/order-success-icon.png"
      />
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
    </div>

    <CartListItem />

    <div v-if="totalPrice" class="flex flex-col gap-4 my-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} Руб.</b>
      </div>

      <div class="flex gap-2">
        <span>Налог 5%</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} Руб.</b>
      </div>
      <button
        :disabled="cartButtonDisabled"
        @click="createOrder"
        class="mt-3 cursor-pointer disabled:bg-slate-300 bg-sky-500 w-full rounded-xl py-3 text-white hover:bg-sky-600 transition active:bg-sky-700"
      >
        Офорить заказ
      </button>
    </div>
  </div>
</template>

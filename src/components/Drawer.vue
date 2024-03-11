<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black/50 z-10"></div>
  <div class="fixed bg-white w-96 h-full right-0 top-0 z-10 p-8">
    <DrawerHead />
    <CartItemList />
    <InfoBlock
      v-if="!totalPrice"
      image-url="/package-icon.png"
      title="Корзина пуста"
      decription="Вы пока не добавили ни одного товара в корзину."
    />
    <div v-else class="flex flex-col gap-4 mt-8">
      <div class="flex gap-4">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} &#x20bd;</b>
      </div>
      <div class="flex gap-4">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} &#x20bd;</b>
      </div>
      <button
        @click="$emit('createOrder')"
        :disabled="totalPrice <= 0"
        class="transition bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-400 cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>

<script setup>
import CartItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
import InfoBlock from './InfoBlock.vue'
defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isCreatingOrder: Boolean
})
defineEmits(['createOrder'])
</script>

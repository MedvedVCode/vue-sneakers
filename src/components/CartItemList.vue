<template>
  <div class="flex flex-col gap-4 flex-1">
    <CartItem
      :item="item"
      v-for="item in cart"
      :key="item.id"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>

<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'
const { cart, removeFromCart } = inject('cart')
</script>

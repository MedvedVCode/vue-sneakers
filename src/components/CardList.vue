<script setup>
import Card from './Card.vue'

const emit = defineEmits(['addToFavorites', 'addToCart'])

defineProps({
  items: Array
})
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :onClickAdd="() => emit('addToCart', item)"
      :onClickFavorite="() => emit('addToFavorites', item)"
      :is-favorite="item.isFavorite"
			:is-added="item.isAdded"
    />
  </div>
</template>

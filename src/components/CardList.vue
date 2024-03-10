<script setup>
import Card from './Card.vue'

const emit = defineEmits(['addToFavorites'])

defineProps({
  items: Array
})
const onClickAdd = () => {
  alert('Добавлено')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :onClickAdd="onClickAdd"
      :onClickFavorite="() => emit('addToFavorites', item)"
      :is-favorite="item.isFavorite"
    />
  </div>
</template>

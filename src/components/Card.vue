<script setup>
defineProps({
	id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
})

</script>

<template>
  <article
    class="relative border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 transition hover:shadow-xl"
  >
    <img
      :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
      alt="Like 1"
      class="absolute"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="Sneaker" />
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400 uppercase">Цена</span>
        <b>{{ price }} &#x20bd;</b>
      </div>
      <img :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="Plus" @click="onClickAdd" />
    </div>
  </article>
</template>

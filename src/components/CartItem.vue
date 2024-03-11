<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="item.imageUrl" :alt="item.title" />
    <div class="flex flex-col flex-1">
      <p>{{ item.title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ item.price }} &#x20bd;</b>
        <img class="opacity-40 hover:opacity-100 transition cursor-pointer" src="/close.svg" alt="Закрыть" @click="emit('onClickRemove', item)"/>
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['onClickRemove'])
	defineProps({
		item: Object
	})
</script>

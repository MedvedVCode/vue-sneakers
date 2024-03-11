<script setup>
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const cart = ref([])

const totalPrice = computed(() => cart.value.reduce((sum, item) => sum + item.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 5) / 100)

const drawerOpen = ref(false)
const isCreatingOrder = ref(false)

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}

const addTofavorites = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      const { data } = await axios.post('https://1af91ffe6c154bf5.mokky.dev/favorites', obj)
      item.isFavorite = true
      item.favoriteId = data.id
    } else {
      await axios.delete(`https://1af91ffe6c154bf5.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false
      item.favoriteId = null
    }
  } catch (e) {
    console.log(e)
  }
}

const addToCart = (item) => {
  item.isAdded = true
  cart.value.push(item)
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://1af91ffe6c154bf5.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value
    })
    cart.value = []
    return data
  } catch (e) {
    console.log(e)
  } finally {
    isCreatingOrder.value = false
  }
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://1af91ffe6c154bf5.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (e) {
    console.log(e)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get('https://1af91ffe6c154bf5.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false,
      favoriteId: null
    }))
  } catch (e) {
    console.log(e)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)
watch(cart, () => items.value.map((item) => (item.isAdded = false)))
watch(cart, () => localStorage.setItem('cart', JSON.stringify(cart.value, 2)), { deep: true })

provide('cart', { closeDrawer, openDrawer, cart, addToCart, removeFromCart })
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :is-creating-order="isCreatingOrder"
  />
  <main class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <section class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="border rounded-md outline-none px-3 py-2">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" alt="Поиск" class="absolute left-3 top-3" />
            <input
              @input="onChangeSearchInput"
              type="text"
              placeholder="Поиск..."
              class="border border-gray-200 rounded-md pl-10 py-2 pr-4 outline-none focus:border-gray-400"
            />
          </div>
        </div>
      </div>
      <CardList
        :items="items"
        class="mt-3"
        @add-to-favorites="addTofavorites"
        @add-to-cart="onClickAddPlus"
      />
    </section>
  </main>
</template>

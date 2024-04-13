<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
  import axios from 'axios'

  import Header from '@/components/Header.vue'
  import CardList from '@/components/CardList.vue'
  import Drawer from '@/components/Drawer.vue'

  const items = ref([]);
  const cart = ref([]);
  const isCreatingOrder = ref(false)

  const drawerOpen = ref(false);

  const totalPrice = computed(
    () => cart.value.reduce((acc, item) => acc + item.price, 0)
  )

  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100 ))

  const closeDrawer = () => {
    drawerOpen.value = false
  }
  const openDrawer = () => {
    drawerOpen.value = true
  }

  const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
  }

  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }

  const createOrder = async () => {
    try {
      isCreatingOrder.value = true
      const { data } = await axios.post(`https://8b0c688acbe64a0b.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: totalPrice.value,
      })

      cart.value = []

      return data;
    } catch (err) {
      console.log(err)
    } finally {
      isCreatingOrder.value = false
    }
  }

  const onClickAddPlus = (item) => {
    if(!item.isAdded) {
      addToCart(item)
    } else {
      removeFromCart(item)
    }
    console.log(cart)
  }

  const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
  })
  const onChangeSelect = event => {
    filters.sortBy = event.target.value
  }
  const onChangeSearchInput = event => {
    filters.searchQuery = event.target.value
  }

  const fetchFavorites = async () => {
    try {
      const { data: favorites } = await axios.get(`https://8b0c688acbe64a0b.mokky.dev/favorites`)

      items.value = items.value.map(item => {
        const favorite = favorites.find(favorite => favorite.parentId === item.id);

        if (!favorite) {
          return item;
        }

        return {
          ...item,
          isFavorite: true,
          favoriteId: favorite.id,
        }
      });
    } catch (err) {
      console.log(err);
    }
  }

  const addToFavorite = async (item) => {
    try {
      if (!item.isFavorite) {
        const obj = {
          parentId: item.id
        };

        item.isFavorite = true

        const { data } = await axios.post(`https://8b0c688acbe64a0b.mokky.dev/favorites`, obj)

        item.isFavorite = true;
        item.favoriteId = data.id;
      } else {
        item.isFavorite = false
        await axios.delete(`https://8b0c688acbe64a0b.mokky.dev/favorites/${item.favoriteId}`)
        item.favoriteId = null
      }

    } catch (err) {
      console.log(err)
    }
  }
  const fetchItems = async () => {
    try {
      const params = {
        sortBy: filters.sortBy,
        // searchQuery: filters.searchQuery,
      }

      if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
      }

      const { data } = await axios.get(
        `https://8b0c688acbe64a0b.mokky.dev/items`,
        {
          params
        }
      )

      items.value = data.map(obj => ({
        ...obj,
        isFavorite: false,
        favoriteId: null,
        isAdded: false
      }));
    } catch (err) {
      console.log(err);
    }
  }

  onMounted(async () => {
    await fetchItems();
    await fetchFavorites();
  })
  watch(filters, fetchItems)

  provide('cart', {
    closeDrawer,
    openDrawer,
    cart,
    addToCart,
    removeFromCart,
  })
</script>

<template>
  <Drawer
    @create-order="createOrder"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    v-if="drawerOpen"
    :is-creating-order="isCreatingOrder.value"
  />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer"/>

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="icon">
            <input
              @input="onChangeSearchInput"
              type="search"
              placeholder="Поиск..."
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
      </div>
    </div>
  </div>
</template>



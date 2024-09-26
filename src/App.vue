<script setup>
import Card from '@/components/Card.vue'
import { reactive, ref, watch } from 'vue'
import axios from 'axios'
import debounce from 'debounce'

const items = ref([])

const filters = reactive({
  page: 1,
  perPage: 9,
  searchQuery: ''
})

const onChangeSelect = event => {
  filters.perPage = event.target.value
}

const onChangeSearchInput = debounce(event => {
  filters.searchQuery = event.target.value
}, 500)

const onChangePageInput = debounce(event => {
  filters.page = Math.max(1, Math.min(event.target.value, items.value.meta.last_page))

}, 500)

const pageUp = () => {
  if (filters.page < items.value.meta.last_page) {
    filters.page++
    fetchItems()
  }
}

const pageDown = () => {
  if (filters.page > 1) {
    filters.page--
    fetchItems()
  }
}

const fetchItems = async () => {
  try {
    const params = {
      page: filters.page,
      per_page: filters.perPage
    }

    if (filters.searchQuery) {
      params.search = `${filters.searchQuery}`
    }
    const { data } = await axios.get('https://api.caiman-app.de/api/cars-test', {
      params
    })
    items.value = data
  } catch (e) {
    console.log(e)
  }
}

fetchItems()
watch(filters, fetchItems)
</script>

<template>
  <div class="flex min-h-[1000px]" :class="items.length === 0 ? 'min-h-[500px]' : 'min-h-0'">
    <div class="min-w-[256px] bg-[#282828] rounded-l-[20px]">
      <div class="text-white font-bold text-2xl pl-14 pt-7">Demo Test</div>
      <div class="text-opacity-45 pt-24 text-white [&>*]:pt-3 [&>*]:pb-3 [&>*]:pl-[30px] [&>*>img]:pr-[30px]">
        <div class="flex"><img src="/vehicles.svg" alt="Profile">Profile</div>
        <div class="flex bg-opacity-20 bg-white text-white"><img src="/profile.svg" alt="Vehicles">Vehicles</div>
        <div class="flex"><img src="/settings.svg" alt="Settings" class="">Settings</div>
      </div>
    </div>
    <div class="w-full">
      <div class="flex items-center justify-between h-[100px] border-b border-[#E4E4E4]">
        <div class="flex flex-wrap">
          <div class="pl-7  text-3xl font-bold text-[#293148]">Vehicles</div>
          <div
            class="bg-[#F3F6F8] text-[#293148] pt-[4px] pr-[14px] pb-[4px] pl-[14px] rounded-md ml-[32px] font-medium">
            256
          </div>
        </div>
        <div class="hidden items-center font-medium text-[#293148] md:flex pr-6">
          <img src="/button_plus.svg" alt="Button_plus">
          <img src="/avatar.png" alt="Avatar" class="pl-10">
          <div class="pl-3">John Doe</div>
          <img src="/uk.png" alt="UK" class="pl-8">
          <div class="pl-2">En</div>
          <img src="/chevron_down.svg" alt="Chevron_down">
        </div>
      </div>
      <div class=" md:flex block">
        <div class="relative pt-6 pl-6 w-full">
          <img class="absolute left-[345px] top-8" src="/search.svg" alt="Search">
          <input @input="onChangeSearchInput"
                 class="placeholder:text-base placeholder:text-[#293148CC] w-[354px] pt-2 pb-2 pl-4 pr-10 mr-8 rounded-md
                      border outline-none focus:border-gray-400"
                 type="text" placeholder="Search VIN">
          <div class="inline-block pr-4">Select vehicles per page:</div>
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none text-[#293148CC]">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
            <option value="4">4</option>
            <option value="5">5</option>
            <option value="6">6</option>
            <option value="7">7</option>
            <option value="8">8</option>
            <option value="9" selected>9</option>
          </select>
        </div>

        <button class="bg-[#D90E32] text-white text-nowrap flex h-[42px] text-center rounded-[10px] ml-6 mr-6 mt-6">
          <span class="text-4xl items-center self-center pl-4 -mt-2">+</span><span
          class="pl-4 pr-4 self-center font-bold text-[12px]">ADD VEHICLE</span>
        </button>

      </div>
      <div class="grid gap-7 p-6" style="grid-template-columns: repeat(auto-fill, minmax(300px, 1fr))">
        <Card
          v-for="item in items.data"
          :key="item.id"
          :id="item.id"
          :item="item"
        />
      </div>
      <div class="text-[#293148CC] pl-[30px] pb-96 pt-9 flex justify-between pr-[30px]">
        <div>Showing {{ items.meta.to }} out of {{ items.meta.total }}</div>
        <div class="flex">
          <img src="/chevron_down.svg" alt="PageLeft"
               @click="pageDown"
               class="rotate-90 cursor-pointer mr-4">
          <input
            @input="onChangePageInput"
            class="placeholder:text-base placeholder:text-[#293148CC] w-[50px] pt-2 pb-2 text-center mr-4 rounded-md
                      border outline-none focus:border-gray-400"
            type="text" :placeholder="filters.page" v-model="filters.page">

          <div class="self-center mr-4">of</div>
          <div
            class="placeholder:text-[#293148CC] text-[#29314873] w-[50px] pt-2 pb-2 text-center mr-4 rounded-md border">
            {{ items.meta.last_page }}
          </div>
          <img src="/chevron_down.svg" alt="PageLeft"
               @click="pageUp"
               class="-rotate-90 cursor-pointer">
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>

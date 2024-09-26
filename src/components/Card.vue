<script setup>
defineProps({
  item: String
})
</script>

<template>
  <div class="p-8 flex bg-[#F3F6F8] rounded-2xl border border-slate-100 cursor-pointer transition hover:-translate-y-2">
    <div class="">
      <img :src="item.placeholder" alt="Preview" class="rounded-md">
      <div class="font-bold size-5 text-[#293148] mt-6 text-nowrap">{{ item.vehicle_name }}</div>
      <div class="font-medium size-4 text-[#29314899] mt-3">{{ item.vin }}</div>
      <div class="border-b  border-b-[#E4E4E4] pt-5"></div>
      <div class="flex justify-between">
      <div class="mt-4 -mb-4 bg-[#EDEDED] text-[#293148CC] font-bold text-sm w-fit h-fit pt-1 pb-1 pr-4 pl-4 rounded-md">5/30</div>
      <div class="pt-4 text-[#29314899] font-medium">3 days left</div>
      </div>
    </div>
  </div>
</template>
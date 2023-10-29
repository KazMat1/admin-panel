<script setup>
import { Bars3Icon } from "@heroicons/vue/24/outline";
import { ref } from "vue";

const show = ref(false);
const toggleShow = () => {
  show.value = !show.value;
};
</script>
<template>
  <div class="relative">
    <div
      class="fixed top-0 w-64 h-screen bg-white z-20 transform"
      :class="{ '-translate-x-full': !show }"
    >
      サイドバー
    </div>
    <div
      class="bg-gray-100 h-screen overflow-hidden"
      :class="{ 'pl-64': show }"
    >
      <div class="bg-white rounded shadow m-4 p-4">
        <Bars3Icon
          class="h-6 w-6 text-gray-600 cursor-pointer"
          @click="toggleShow"
        />
      </div>
      <div>
        <slot />
      </div>
    </div>
  </div>
</template>

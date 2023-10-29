<script setup>
import { Bars3Icon } from "@heroicons/vue/24/outline";
import { onMounted, onUnmounted, ref } from "vue";
import { debounce } from 'lodash';

const innerWidth = ref(window.innerWidth);
const show = ref(innerWidth.value >= 1280 ? true : false);
const toggleShow = () => {
  show.value = !show.value;
};

const checkWindowSize = () => {
  if (window.innerWidth >= 1280) {
    if (show.value === false && innerWidth.value < 1280) show.value = true;
  } else {
    if (show.value === true) show.value = false;
  }
  innerWidth.value = window.innerWidth;
};
onMounted(() => {
  // resizeイベントで、windowサイズの変更を検知する。debounceで、0.1秒経過して、チェック
  window.addEventListener('resize', debounce(checkWindowSize, 100));
});
onUnmounted(() => {
  window.removeEventListener('resize', checkWindowSize);
});
</script>
<template>
  <div class="relative">
    <div
      class="fixed top-0 w-64 h-screen bg-white z-20 transform duration-300"
      :class="{ '-translate-x-full': !show }"
    >
      サイドバー
    </div>
    <div
      class="fixed xl:hidden inset-0 bg-gray-900 opacity-50 z-10 duration-300"
      @click="toggleShow"
      v-show="show"
    ></div>
    <div
      class="bg-gray-100 h-screen overflow-hidden"
      :class="{ 'xl:pl-64': show }"
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

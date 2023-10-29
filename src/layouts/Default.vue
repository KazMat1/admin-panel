<script setup>
import { Bars3Icon, SunIcon, MoonIcon } from "@heroicons/vue/24/outline";
import { onMounted, onUnmounted, ref } from "vue";
import { debounce } from "lodash";

const innerWidth = ref(window.innerWidth);
const show = ref(innerWidth.value >= 1280 ? true : false);
const toggleShow = () => {
  show.value = !show.value;
};

const theme = ref("light");

// check the theme a user select and add class for theme to html element
if (localStorage.theme === 'dark') {
  document.documentElement.classList.add('dark');
  theme.value = 'dark';
} else {
  document.documentElement.classList.remove('dark');
  theme.value = 'light';
}

// change theme color method
const changeMode = (mode) => {
  theme.value = mode
  // add class to html element
  theme.value === 'light'
    ? document.documentElement.classList.remove('dark')
    : document.documentElement.classList.add('dark');
  // set a parameter in local storage
    localStorage.theme = mode;
}

// check window size method
const checkWindowSize = () => {
  if (window.innerWidth >= 1280) {
    if (show.value === false && innerWidth.value < 1280) show.value = true;
  } else {
    if (show.value === true) show.value = false;
  }
  innerWidth.value = window.innerWidth;
};
onMounted(() => {
  // detect the size of window from resize event. check it with debounce method per 0.1s
  window.addEventListener("resize", debounce(checkWindowSize, 100));
});
onUnmounted(() => {
  // not required to check the window size because of unmounted
  window.removeEventListener("resize", checkWindowSize);
});
</script>
<template>
  <div class="relative">
    <div
      class="fixed top-0 w-64 h-screen bg-white dark:bg-gray-800 z-20 transform duration-300 dark:text-gray-300"
      :class="{ '-translate-x-full': !show }"
    >
      サイドバー
    </div>
    <div
      class="fixed xl:hidden inset-0 bg-gray-900 z-10 opacity-50"
      @click="toggleShow"
      v-show="show"
    ></div>
    <div
      class="bg-gray-100 dark:bg-gray-900 h-screen overflow-hidden duration-300"
      :class="{ 'xl:pl-64': show }"
    >
      <div
        class="flex items-center justify-between bg-white dark:bg-gray-800 rounded shadow m-4 p-4"
      >
        <Bars3Icon
          class="h-6 w-6 text-gray-600 dark:text-gray-300 cursor-pointer"
          @click="toggleShow"
        />
        <div>
          <MoonIcon
            class="w-7 h-7 text-gray-600 cursor-pointer"
            @click="changeMode('dark')"
            v-if="theme === 'light'"
          />
          <SunIcon
            class="w-7 h-7 text-gray-300 cursor-pointer"
            @click="changeMode('light')"
            v-else
          />
        </div>
      </div>
      <div class="dark:text-gray-300">
        <slot />
      </div>
    </div>
  </div>
</template>

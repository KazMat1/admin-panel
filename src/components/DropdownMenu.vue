<script setup>
import { ArrowRightOnRectangleIcon, UserIcon } from "@heroicons/vue/24/outline";
import { onMounted, onUnmounted, ref } from "vue";

const show = ref(false);
const toggleShow = () => {
  show.value = !show.value;
};

const root = ref(null);

// hide the dropdown when click outside itself
const clickOutside = (e) => {
  // click the area excluded the dropdown or the avator icon and dropdown show up now
  if (!root.value.contains(e.target) && show.value) {
    show.value = false;
  }
};
onMounted(() => document.addEventListener("click", clickOutside));
onUnmounted(() => document.removeEventListener("click"), clickOutside);
</script>

<template>
  <div class="relative" ref="root">
    <img
      src="../assets/images/avatar.png"
      class="rounded-full w-10 h-10 cursor-pointer"
      @click="toggleShow"
    />
    <transition
      enter-active-class="transition duration-300"
      enter-from-class="transform opacity-0 -translate-y-2 "
      leave-active-class="transition duration-300"
      leave-to-class="transform opacity-0 -translate-y-2"
    >
      <div
        class="absolute top-16 right-0 z-10 w-40 py-2 bg-white rounded-sm shadow dark:bg-gray-800"
        v-show="show"
      >
        <ul>
          <li
            class="text-gray-700 dark:text-gray-300 hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-blue-600 dark:hover:text-blue-600 p-2"
          >
            <router-link to="/profile" class="flex items-center space-x-2">
              <UserIcon class="w-5 h-5" />
              <span class="text-sm font-bold">プロフィール</span></router-link
            >
          </li>
          <li
            class="text-gray-700 dark:text-gray-300 hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-blue-600 dark:hover:text-blue-600 p-2"
          >
            <router-link to="/#" class="flex items-center space-x-2">
              <ArrowRightOnRectangleIcon class="w-5 h-5" />
              <span class="text-sm font-bold">ログアウト</span></router-link
            >
          </li>
        </ul>
      </div>
    </transition>
  </div>
</template>

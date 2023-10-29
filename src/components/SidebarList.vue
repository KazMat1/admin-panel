<script setup>
import { reactive } from "vue";
import {
  ChartBarIcon,
  ShoppingCartIcon,
  ChevronDownIcon,
} from "@heroicons/vue/24/outline";

const icons = {
  ChartBarIcon: ChartBarIcon,
  ShoppingCartIcon: ShoppingCartIcon,
  ChevronDownIcon: ChevronDownIcon,
};

const lists = reactive([
  {
    name: "ダッシュボード",
    icon: "ChartBarIcon",
    link: "/",
  },
  {
    name: "EC",
    icon: "ShoppingCartIcon",
    link: "/#",
    show: false,
    sublists: [
      {
        name: "商品一覧",
        link: "/#",
      },
      {
        name: "注文一覧",
        link: "/#",
      },
    ],
  },
]);

const toggleSublist = (name) => {
  const list = lists.find((list) => list.name === name);
  list.show = !list.show;
};

const enter = (element) => {
  element.style.height = 'auto';
  const height = getComputedStyle(element).height;
  element.style.height = 0;
  getComputedStyle(element);
  setTimeout(() => {
    element.style.height = height;
  });
};

const leave = (element) => {
  element.style.height = getComputedStyle(element).height;
  getComputedStyle(element);
  setTimeout(() => {
    element.style.height = 0;
  });
};
</script>
<template>
  <ul class="text-gray-700">
    <li class="mb-1" v-for="list in lists" :key="list.name">
      <a
        v-if="!list.sublists"
        :href="list.link"
        class="flex items-center p-2 rounded-sm text-gray-700 dark:text-gray-300 hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-blue-600 dark:hover:text-blue-600"
      >
        <component :is="icons[list.icon]" class="w-6 h-6 mr-2"></component>
        <span>{{ list.name }}</span>
      </a>
      <div
        v-else
        class="flex items-center justify-between p-2 cursor-pointer rounded-sm text-gray-700 dark:text-gray-300 hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-blue-600 dark:hover:text-blue-600"
        @click="toggleSublist(list.name)"
      >
        <div class="flex items-center">
          <component :is="icons[list.icon]" class="w-6 h-6 mr-2"></component>
          <span>{{ list.name }}</span>
        </div>
        <ChevronDownIcon class="w-4 h-4" />
      </div>
      <transition @enter="enter" @leave="leave">
        <ul class="mt-1 overflow-hidden" v-show="list.show">
          <li class="mb-1" v-for="list in list.sublists" :key="list.name">
            <a
              :href="list.link"
              class="block p-2 rounded-sm text-gray-700 dark:text-gray-300 hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-blue-600 dark:hover:text-blue-600"
            >
              <span class="pl-8">{{ list.name }}</span>
            </a>
          </li>
        </ul>
      </transition>
    </li>
  </ul>
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: height 0.3s;
}
</style>

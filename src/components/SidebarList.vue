<script setup>
import { reactive } from 'vue';

const lists = reactive([
  {
    name: 'ダッシュボード',
    icon: 'TemplateIcon',
    link: '/',
  },
  {
    name: 'EC',
    icon: 'ShoppingCartIcon',
    link: '/#',
    sublists: [
      {
        name: '商品一覧',
        link: '/#',
      },
      {
        name: '注文一覧',
        link: '/#',
      },
    ],
  },
]);
</script>
<template>
  <ul class="text-gray-700">
    <li class="mb-1" v-for="list in lists" :key="list.name">
      <!-- <a
        :href="list.link"
        class="block p-2 rounded-sm hover:text-white hover:bg-blue-400"
      > -->
      <a
        :href="list.link"
        class="block p-2 rounded-sm text-gray-700 dark:text-gray-300 hover:bg-blue-100 dark:hover:bg-gray-700 hover:text-blue-600 dark:hover:text-blue-600"
      >
        <span>{{ list.name }}</span>
      </a>
    </li>
  </ul>
</template>

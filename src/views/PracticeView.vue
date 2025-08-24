<template>
  <div id="app" class="container py-4">
    <div class="row">
      <!-- 商品列表區 -->
      <ProductList :product-list="products" @add-to-cart="handleAddToCart" />
      <!-- 購物車區 -->
      <ProductCart :cart="cart" @remove-cart-item="handleRemoveCartItem" />
    </div>

    <!-- 通知元件 -->
    <ProductNotify />
    {{ message }}
  </div>
</template>
<script setup>
import ProductCart from '@/components/practice/ProductCart.vue';
import ProductList from '@/components/practice/ProductList.vue';
import ProductNotify from '@/components/practice/ProductNotify.vue';

import { ref, provide, reactive } from 'vue';
const cart = ref([]);
const notify = reactive({ message: '' });

provide('notify', notify);
provide('removeNotifyMessage', () => {
  notify.message = '';
});
// 需求功能描述
// 請嘗試將「商品列表」、「購物車」、「通知」拆分成 3 個元件
// 請使用 props 將商品資料傳遞至「商品列表」元件       ** ok
//「購物車」元件的刪除功能，請使用 emit 傳遞事件       ** ok
// 使用 provide, inject 完成通知功能

const products = [
  {
    id: 1,
    title: '耳罩式藍牙耳機',
    description: '舒適配戴，支援降噪技術',
    price: 2490,
    imageUrl:
      'https://plus.unsplash.com/premium_photo-1677838847804-4054143fb91a?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8OXx8ZWFycGhvbmV8ZW58MHx8MHx8fDA%3D',
  },
  {
    id: 2,
    title: '耳罩式彩虹耳機',
    description: '舒適配戴，支援降噪技術',
    price: 1380,
    imageUrl:
      'https://images.unsplash.com/photo-1505236273191-1dce886b01e9?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTV8fGVhcnBob25lfGVufDB8fDB8fHww',
  },
  {
    id: 3,
    title: '時尚藍牙耳機',
    description: '舒適配戴，支援降噪技術',
    price: 7990,
    imageUrl:
      'https://images.unsplash.com/photo-1496957961599-e35b69ef5d7c?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8OHx8ZWFycGhvbmV8ZW58MHx8MHx8fDA%3D',
  },
  {
    id: 4,
    title: '機械式鍵盤',
    description: '紅軸機械鍵盤，打字手感極佳',
    price: 1890,
    imageUrl:
      'https://plus.unsplash.com/premium_photo-1679913792906-13ccc5c84d44?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTN8fGVhcnBob25lfGVufDB8fDB8fHww',
  },
  {
    id: 5,
    title: '無線滑鼠',
    description: '靜音按鍵設計，長效電池',
    price: 890,
    imageUrl:
      'https://images.unsplash.com/photo-1484704849700-f032a568e944?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTl8fGVhcnBob25lfGVufDB8fDB8fHww',
  },
];
const handleAddToCart = (product) => {
  const index = cart.value.findIndex((cartItem) => cartItem.id === product.id);
  if (index < 0) {
    cart.value.push(product);
    addNotifyMessage(`${product.title}已加入購物車`);
    return;
  }
  addNotifyMessage(`${product.title}加入購物車失敗`);
};
const handleRemoveCartItem = (product) => {
  const index = cart.value.findIndex((cartItem) => cartItem.id === product.id);
  cart.value.splice(index, 1);
  addNotifyMessage(`${product.title}已從購物車中移除`);
};
const addNotifyMessage = (msg) => {
  notify.message = msg;
  setTimeout(() => {
    notify.message = '';
  }, 2000);
};
</script>
<style>
body {
  background: #f2f2f2f2;
}

.card-img-top {
  height: 150px;
  object-fit: cover;
}
</style>

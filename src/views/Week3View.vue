<template>
  <div id="root">
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-4">
          <DrinkMenu :drink-menu="data" @add-to-cart="handleAddToCart" />
        </div>
        <div class="col-md-8">
          <Cart
            :cart="cart"
            :cartTotalPrice="cartTotalPrice"
            @create-order="handleCreateOrder"
            @remove-cart-item="handleRemoveCartItem"
            v-model="comment"
          />
        </div>
      </div>
      <hr />
      <div class="row justify-content-center">
        <div class="col-8">
          <Order :order="order" />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
// Level	說明
// （原版）LV1	僅將以上功能完成，不拆分成元件（可以省略：刪除、調整數量的功能）
// （原版）LV2	包含以上功能，至少額外拆分兩個元件
// （原版挑戰）LV3	不參考解答製作菜單工具，不省略任何功能
// 且至少包含額外三個元件
// （重複品項無法重複加入）

// 【題目】： 餐點管理工具

// 作業使用者故事：

// 點餐人員可以將左側的品項加入購物車
// 點餐人員，可以刪除、調整購物車品項數量（Level 1, 2 可不做）
// 點餐人員可加入備註
// 點餐人員可以建立訂單
import { computed, ref } from 'vue';
import DrinkMenu from '@/components/week3/DrinkMenu.vue';
import Order from '@/components/week3/Order.vue';
import Cart from '@/components/week3/Cart.vue';

const data = [
  {
    id: 1,
    name: '珍珠奶茶',
    description: '香濃奶茶搭配QQ珍珠',
    price: 50,
  },
  {
    id: 2,
    name: '冬瓜檸檬',
    description: '清新冬瓜配上新鮮檸檬',
    price: 45,
  },
  {
    id: 3,
    name: '翡翠檸檬',
    description: '綠茶與檸檬的完美結合',
    price: 55,
  },
  {
    id: 4,
    name: '四季春茶',
    description: '香醇四季春茶，回甘無比',
    price: 45,
  },
  {
    id: 5,
    name: '阿薩姆奶茶',
    description: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
  },
  {
    id: 6,
    name: '檸檬冰茶',
    description: '檸檬與冰茶的清新組合',
    price: 45,
  },
  {
    id: 7,
    name: '芒果綠茶',
    description: '芒果與綠茶的獨特風味',
    price: 55,
  },
  {
    id: 8,
    name: '抹茶拿鐵',
    description: '抹茶與鮮奶的絕配',
    price: 60,
  },
];
const cart = ref([]);
const cartTotalPrice = computed(() => {
  return cart.value.reduce((acc, cartItem) => {
    return (acc += cartItem.price * cartItem.count);
  }, 0);
});
const order = ref({});
const comment = ref('');
const handleAddToCart = (drink) => {
  const index = cart.value.findIndex((cartItem) => cartItem.id === drink.id);
  if (index < 0) {
    drink.count = 1;
    cart.value.push(drink);
  }
};
const handleRemoveCartItem = (removeId) => {
  const index = cart.value.findIndex((cartItem) => cartItem.id === removeId);
  cart.value.splice(index, 1);
};
const handleCreateOrder = () => {
  order.value.orderList = cart.value;
  order.value.comment = comment.value;
  order.value.totalPrice = cartTotalPrice.value;
};
</script>
<style></style>

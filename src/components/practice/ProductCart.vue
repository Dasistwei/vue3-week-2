<template>
  <div class="col-md-4">
    <h2 class="mb-3">購物車</h2>
    <ul class="list-group mb-3">
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
        v-for="cartItem in cart"
        :key="cartItem.id"
      >
        <div>
          <h6 class="my-0">{{ cartItem.title }}</h6>
          <small class="text-muted">數量：1</small>
        </div>
        <div>
          <span class="text-muted">${{ cartItem.price }}</span>
          <button class="btn btn-sm btn-outline-danger ms-2" @click="removeCartItem(cartItem)">
            移除
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>
<script setup>
defineProps({
  cart: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(['remove-cart-item']);
const removeCartItem = (cartItem) => {
  emit('remove-cart-item', cartItem);
};
</script>

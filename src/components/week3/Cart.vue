<template>
  <table class="table">
    <thead>
      <tr>
        <th scope="col" width="50">操作</th>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col" width="90">數量</th>
        <th scope="col">單價</th>
        <th scope="col">小計</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cartItem in cart" :key="cartItem.id">
        <td>
          <button type="button" class="btn btn-sm" @click="removeCartItem(cartItem.id)">x</button>
        </td>
        <td>{{ cartItem.name }}</td>
        <td>
          <small>{{ cartItem.description }}</small>
        </td>
        <td>
          <select class="form-select" v-model.number="cartItem.count">
            <option :value="number" v-for="number in 10" :key="number">
              {{ number }}
            </option>
          </select>
        </td>
        <td>{{ cartItem.price }}</td>
        <td>{{ cartItem.price * cartItem.count }}</td>
      </tr>
    </tbody>
  </table>
  <div class="text-end mb-3">
    <h5>
      總計: <span>${{ cartTotalPrice }}</span>
    </h5>
  </div>
  <textarea
    class="form-control mb-3"
    rows="3"
    placeholder="備註"
    :value="modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  ></textarea>
  <div class="text-end">
    <button class="btn btn-primary" @click="createOrder">送出</button>
  </div>
</template>
<script setup>
defineProps({
  cart: {
    type: Array,
    required: true,
  },
  cartTotalPrice: {
    type: Number,
    required: true,
  },
  modelValue: {
    type: String,
    required: true,
  },
});
const emit = defineEmits(['create-order', 'update:modelValue', 'remove-cart-item']);

const createOrder = () => {
  // emit('create-order', drink);
  emit('create-order');
};
const removeCartItem = (cartItemId) => {
  // emit('create-order', drink);
  emit('remove-cart-item', cartItemId);
};
</script>
<style scoped></style>

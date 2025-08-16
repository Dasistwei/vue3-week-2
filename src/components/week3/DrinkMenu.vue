<template>
  <div class="list-group">
    <a
      href="#"
      class="list-group-item list-group-item-action"
      v-for="drink in drinkMenu"
      :key="drink.id"
      @click="addToCart(drink)"
    >
      <div class="d-flex w-100 justify-content-between">
        <h5 class="mb-1">{{ drink.title }}</h5>
        <small>${{ drink.price }}</small>
      </div>
      <p class="mb-1">{{ drink.description }}</p>
    </a>
  </div>
</template>
<script setup>
defineProps({
  drinkMenu: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(['add-to-cart']);

const addToCart = (drink) => {
  emit('add-to-cart', drink);
};
</script>
<style scoped></style>

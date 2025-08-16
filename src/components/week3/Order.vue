<template>
  <div class="card">
    <div class="card-body">
      <div class="card-title">
        <h5>訂單</h5>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">品項</th>
              <th scope="col">數量</th>
              <th scope="col">小計</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="orderItem in order.orderList" :key="orderItem">
              <td>{{ orderItem.name }}</td>
              <td>{{ orderItem.count }}</td>
              <td>{{ orderItem.price * orderItem.count }}</td>
            </tr>
          </tbody>
        </table>
        <div class="text-end">
          備註: <span>{{ order.comment }}</span>
        </div>
        <div class="text-end">
          <h5>
            總計: <span>${{ order.totalPrice }}</span>
          </h5>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
defineProps({
  order: {
    type: Object,
    required: true,
  },
});
</script>
<style scoped></style>

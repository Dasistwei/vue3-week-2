<template>
  <h1>This is an review page</h1>
  <div class="review">
    <input type="text" v-model="newName" />
    {{ newName }}
    <input type="text" v-model="newNumber" />
    {{ newNumber }}
    <button type="button" @click="addProduct">add to list</button>
    <table>
      <thead>
        <tr>
          <th>title</th>
          <th>price</th>
          <th>adjust price</th>
          <th>delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.price }}</td>
          <td><input type="number" v-model="item.price" /></td>
          <td><button type="button" @click="deleteItem(item.id)">delete</button></td>
        </tr>
      </tbody>
    </table>
    total: {{ sum }}
  </div>
</template>
<script setup>
import { computed, onMounted, ref } from 'vue';
import axios from 'axios';

const newName = ref('');
const newNumber = ref(0);
// const data = ref([
//   { id: 1, name: '珍珠奶茶', price: 50 },
//   { id: 2, name: '冬瓜檸檬', price: 45 },
//   { id: 3, name: '翡翠檸檬', price: 55 },
//   { id: 4, name: '四季春茶', price: 45 },
//   { id: 5, name: '阿薩姆奶茶', price: 50 },
//   { id: 6, name: '檸檬冰茶', price: 45 },
//   { id: 7, name: '芒果綠茶', price: 55 },
//   { id: 8, name: '抹茶拿鐵', price: 60 },
// ]);
const data = ref([]);
const addProduct = () => {
  console.log('add product');
  data.value.push({
    id: new Date(),
    name: newName.value,
    price: newNumber.value,
  });
  newName.value = '';
  newNumber.value = 0;
};
const deleteItem = (id) => {
  console.log(id);
  const index = data.value.findIndex((item) => item.id === id);
  data.value.splice(index, 1);
};
const sum = computed(() => {
  let sum = 0;
  data.value.forEach((item) => {
    sum += item.price * 1;
  });
  return sum;
});
onMounted(() => {
  setTimeout(() => {
    data.value = [
      { id: 1, name: '珍珠奶茶', price: 50 },
      { id: 2, name: '冬瓜檸檬', price: 45 },
      { id: 3, name: '翡翠檸檬', price: 55 },
      { id: 4, name: '四季春茶', price: 45 },
      { id: 5, name: '阿薩姆奶茶', price: 50 },
      { id: 6, name: '檸檬冰茶', price: 45 },
      { id: 7, name: '芒果綠茶', price: 55 },
      { id: 8, name: '抹茶拿鐵', price: 60 },
    ];
  }, 2000);
});
async function getData() {
  const response = await axios.get('https://randomuser.me/api/');
  console.log(response);
}
getData();
</script>
<style></style>

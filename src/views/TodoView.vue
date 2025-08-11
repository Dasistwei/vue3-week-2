<template>
  <h1>This is an todo page</h1>
  <h2>register</h2>
  <input type="email" placeholder="Email" v-model="signupField.email" />
  <input type="text" placeholder="password" v-model="signupField.password" />
  <input type="text" placeholder="nickName" v-model="signupField.nickname" />
  <br />
  {{ signupField }}
  <button type="button" @click="signup">sign up</button>
  uid: {{ signupRes }}
  <br />
  <h2>sign in</h2>
  <input type="email" placeholder="Email" v-model="signInField.email" />
  <input type="text" placeholder="password" v-model="signInField.password" />
  <br />
  {{ signInField }}
  <button type="button" @click="signin">sign in</button>
  token: {{ signInRes }}

  <h2>validate</h2>
  {{ user }}
  <div class="" v-if="user.uid">
    <p>{{ user.nickname }}</p>
    <p>{{ user.uid }}</p>
  </div>
  <div class="" v-else>you're not log in yet</div>

  <div class="review">
    <h2>todo</h2>
    <!-- {{ todos }} -->
    <br />
    <input type="text" v-model="newContent" />
    {{ newContent }}
    <button type="button" @click="addTodo">add todo</button>
    <!-- <table>
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
    </table> -->
    <table>
      <thead>
        <tr>
          <!-- <th>id</th> -->
          <th>content</th>
          <th>status</th>
          <th>delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="todo in todos" :key="todo.id">
          <!-- <td>{{ todo.id }}</td> -->
          <td v-if="!isUpdate">{{ todo.content }}</td>
          <td v-else>
            <input type="text" v-model="todo.content" />
          </td>
          <td>{{ todo.status }}</td>
          <td v-if="!isUpdate">
            <button type="button" @click="showUpdateInput">update</button>
          </td>
          <td v-else>
            <button type="button" @click="updateTodo(todo.id, todo.content)">confirm update</button>
          </td>
          <td><button type="button" @click="deleteTodo(todo.id)">delete</button></td>
          <td><button type="button" @click="toggleTodo(todo.id)">toggle status</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script setup>
import { computed, onMounted, ref } from 'vue';
import axios from 'axios';
const api = 'https://todolist-api.hexschool.io/';
let apiClient = axios.create({ baseURL: api });

const signupField = ref({
  email: '',
  password: '',
  nickname: '',
});
const signInField = ref({
  email: '',
  password: '',
});
const signupRes = ref('');
const signInRes = ref('');
const user = ref({
  nickname: '',
  uid: '',
});
const token = ref('');
const signup = async () => {
  try {
    const res = await axios.post(`${api}users/sign_up`, signupField.value);
    console.log(res.data.uid);
    signupRes.value = res.data.uid;
    // {"status":true,"uid":"-OXNSxJr0nQ4w08HS89i"}
  } catch (error) {
    console.log('error', error);
  }
};
const signin = async () => {
  try {
    const res = await axios.post(`${api}users/sign_in`, signInField.value);
    console.log(res);
    signInRes.value = res.data.token;

    document.cookie = `customTodoToken=${res.data.token};path=/`;
  } catch (error) {
    console.log('error', error);
  }
};

onMounted(async () => {
  token.value = document.cookie.replace(/(?:^|.*;\s*)customTodoToken\s*=\s*([^;]*).*$/i, '$1');

  apiClient.interceptors.request.use((config) => {
    config.headers.Authorization = token.value; // 確保每次請求用最新的 token
    return config;
  });

  const res = await apiClient.get(`users/checkout`);
  user.value = res.data;
});

//

const newContent = ref('');

const data = ref([]);
const todos = ref([]);
const isUpdate = ref(false);

const addTodo = async () => {
  try {
    await apiClient.post('todos', { content: newContent.value });
    newContent.value = '';
    getTodos();
  } catch (error) {
    console.log('get todos error', error);
  }
};
const showUpdateInput = () => {
  isUpdate.value = !isUpdate.value;
};
const updateTodo = async (id, updateContent) => {
  try {
    await apiClient.put(`todos/${id}`, { content: updateContent });
    isUpdate.value = false;
    getTodos();
  } catch (error) {
    console.log('update todos error', error);
  }
};
const deleteTodo = async (id) => {
  try {
    await apiClient.delete(`todos/${id}`);

    getTodos();
  } catch (error) {
    console.log('update todos error', error);
  }
};
const toggleTodo = async (id) => {
  try {
    await apiClient.patch(`todos/${id}/toggle`);
    getTodos();
  } catch (error) {
    console.log('update todos error', error);
  }
};
const sum = computed(() => {
  let sum = 0;
  data.value.forEach((item) => {
    sum += item.price * 1;
  });
  return sum;
});
onMounted(() => {
  getTodos();
});
async function getTodos() {
  try {
    const token = document.cookie.replace(/(?:^|.*;\s*)customTodoToken\s*=\s*([^;]*).*$/i, '$1');
    const response = await axios.get(`${api}todos`, {
      headers: {
        Authorization: token,
      },
    });
    console.log(response.data);
    todos.value = response.data.data;
  } catch (error) {
    console.log('get todos error', error);
  }
}
</script>
<style></style>

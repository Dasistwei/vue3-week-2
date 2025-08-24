<template>
  <div class="position-fixed top-0 end-0 p-3" style="z-index: 1050" v-if="notify.message">
    <div class="toast show align-items-center text-white bg-success border-0">
      <div class="d-flex">
        <div class="toast-body">{{ notify.message }}</div>
        <button
          type="button"
          class="btn-close btn-close-white me-2 m-auto"
          @click="handleClick"
        ></button>
      </div>
    </div>
  </div>
</template>
<script setup>
import { inject } from 'vue';
const notify = inject('notify');
const removeNotifyMessage = inject('removeNotifyMessage');
const handleClick = () => {
  removeNotifyMessage();
};
</script>

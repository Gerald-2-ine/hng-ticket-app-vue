<template>
  <div class="toast" :class="type">
    <p>{{ message }}</p>
  </div>
</template>

<script setup>
import { onMounted, onBeforeUnmount } from "vue";

const props = defineProps({
  message: String,
  type: String,
});
const emit = defineEmits(["close"]);

let timer;
onMounted(() => {
  timer = setTimeout(() => emit("close"), 3000);
});
onBeforeUnmount(() => clearTimeout(timer));
</script>

<style scoped>
@import "../styles/toast.css";
</style>

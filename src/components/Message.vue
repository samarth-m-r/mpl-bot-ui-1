
<template>
  <div class="flex gap-2 my-1 p-2">
    <span v-if="isUser">👤</span>
    <img
      v-else
      src="../assets/Maersk_Oil_logo.svg_-350x316.png"
      alt="MPL Bot Logo"
      class="w-5 h-5 mt-1"
    />
    <div class="bg-gray-100 p-2 rounded-lg w-fit max-w-md">
      <div class="font-bold">{{ isUser ? "You" : "MPL Bot" }}</div>
      <div>{{ content }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  content: string;
  role: string;
}>();

const isUser = computed(() => props.role === "user");
</script>
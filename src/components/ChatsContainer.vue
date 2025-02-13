
<template>
  <div ref="chatsContainer" class="overflow-auto">
    <Message
      v-for="(chat, index) in CHATS"
      :key="index"
      :content="chat.content"
      :role="chat.role"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, nextTick } from "vue";
import Message from "./Message.vue";
import { CHATS } from "@/stores/chat";

const chatsContainer = ref<HTMLElement | null>(null);

nextTick(() => {
  chatsContainer.value?.scrollTo(0, chatsContainer.value.scrollHeight);
});
</script>
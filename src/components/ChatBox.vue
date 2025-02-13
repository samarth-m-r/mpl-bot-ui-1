<template>
  <div class="flex gap-1">
    <div class="flex-1 relative border-2 border-gray-300 p-3 rounded-lg flex">
      <input
        v-model="message"
        type="text"
        class="w-full outline-none"
        placeholder="Type or speak a message..."
        @keyup.enter="sendChats"
      />
      <div class="flex gap-2">
        <button
          class="border-gray-300 border px-2 py-1 rounded"
          @click="toggleVoiceInput"
        >
          <span v-if="isUserVoiceEnabled">🎤</span>
          <span v-else>🔇</span>
        </button>
        <button class="border-gray-300 border px-2 py-1 rounded" @click="sendChats">
          Send
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, nextTick } from "vue";
import { CHATS } from "@/stores/chat";
import { useSpeechRecognition } from "@vueuse/core";

const message = ref("");
const isUserVoiceEnabled = ref(false);

const { isListening, result, start, stop, isSupported } = useSpeechRecognition({
  lang: "en-US",
  continuous: false,
});

watch(result, (value) => {
  if (isUserVoiceEnabled.value) {
    message.value = value;
  }
});

function toggleVoiceInput() {
  isUserVoiceEnabled.value = !isUserVoiceEnabled.value;
  isUserVoiceEnabled.value ? start() : stop();
}

function sendChats() {
  if (!message.value.trim()) return;

  // Push user message
  CHATS.value.push({ role: "user", content: message.value });

  // Clear input field
  message.value = "";

  // Auto-scroll to the latest message
  nextTick(() => {
    const chatContainer = document.querySelector(".overflow-auto");
    chatContainer?.scrollTo(0, chatContainer.scrollHeight);
  });

  // Simulate bot response
  setTimeout(() => {
    CHATS.value.push({ role: "bot", content: "This is a dummy response." });

    // Auto-scroll after response
    nextTick(() => {
      const chatContainer = document.querySelector(".overflow-auto");
      chatContainer?.scrollTo(0, chatContainer.scrollHeight);
    });
  }, 1000);
}
</script>
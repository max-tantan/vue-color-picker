<script setup>

</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-900 text-white">
    <div class="bg-gray-800 p-6 rounded-2xl shadow-xl w-80 text-center">

      <h1 class="text-xl font-bold mb-4">🎨 Color Picker</h1>

      <!-- Preview -->
      <div
        :style="{ backgroundColor: color }"
        class="w-full h-32 rounded-xl mb-4 border border-gray-600"
      ></div>

      <!-- Input -->
      <input
        type="color"
        v-model="color"
        class="w-full h-12 cursor-pointer mb-4"
      />

      <!-- HEX Code -->
      <p class="mb-3 text-lg font-mono">{{ color }}</p>

      <!-- Copy Button -->
      <button
        @click="copyColor"
        class="bg-blue-500 hover:bg-blue-600 px-4 py-2 rounded-lg w-full transition"
      >
        Copy HEX
      </button>

      <!-- Notification -->
      <p v-if="copied" class="text-green-400 mt-2 text-sm">
        Copied!
      </p>

    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const color = ref("#6366f1");
const copied = ref(false);

const copyColor = async () => {
  await navigator.clipboard.writeText(color.value);
  copied.value = true;

  setTimeout(() => {
    copied.value = false;
  }, 1500);
};
</script>

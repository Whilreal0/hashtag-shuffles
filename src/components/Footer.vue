<script setup>
import { ref } from 'vue';

const currentYear = new Date().getFullYear();
</script>

<template>
  <div class="bg-blue-300 text-center px-2 py-2 md:px-[17rem]">
    <p class="text-sm">
        &copy; {{ currentYear }} Whilreal. All rights reserved.
      </p>
  </div>
</template>
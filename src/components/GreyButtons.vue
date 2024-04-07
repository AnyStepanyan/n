<template>
  <v-btn
  class="mr-2" 
  variant="tonal"
  @click="$emit('eventHandler')"
  >
   {{buttonName}} 
   </v-btn>
</template>

<script setup>
import { defineProps } from 'vue';
defineProps({
buttonName: String
})
</script>

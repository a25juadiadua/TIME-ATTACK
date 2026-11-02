<script setup>
defineProps({
  tecles: Array,
  bloquejat: Boolean
})

const emit = defineEmits(['premuda']);
</script>

<template>
  <div class="d-flex justify-content-center gap-3 flex-wrap">

    <button
      v-for="tecla in tecles"
      :key="tecla"
      :disabled="bloquejat"
      @click="emit('premuda', tecla)"
      class="btn btn-dark btn-lg fw-bold rounded-4 shadow px-4 py-3"
      style="min-width: 75px; font-size: 1.5rem;"
    >
      {{ tecla }}
    </button>

  </div>
</template>
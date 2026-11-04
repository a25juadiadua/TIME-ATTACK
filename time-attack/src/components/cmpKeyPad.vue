<script setup>
defineProps({
  tecles: Array,
  bloquejat: Boolean,
  premudes: Array
})

const emit = defineEmits(['premuda']);
</script>

<template>
  <div class="d-flex justify-content-center gap-3 flex-wrap">

    <button
      v-for="tecla in tecles"
      :key="tecla"
      :disabled="bloquejat || premudes.includes(tecla)"
      @click="emit('premuda', tecla)"
      class="btn btn-lg fw-bold rounded-4 shadow px-4 py-3 tecla-btn"
      :class="{
  'btn-primary': tecla === 'A',
  'btn-success': tecla === 'S',
  'btn-warning': tecla === 'D',
  'btn-danger': tecla === 'F'
}"
    >
      {{ tecla }}
    </button>

  </div>
</template>

<style scoped>
.tecla-btn {
  min-width: 85px;
  font-size: 1.6rem;
  transition: transform 0.2s, box-shadow 0.2s;
}

.tecla-btn:not(:disabled):hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2) !important;
}

.tecla-btn:not(:disabled):active {
  transform: scale(0.95);
}

.tecla-btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>
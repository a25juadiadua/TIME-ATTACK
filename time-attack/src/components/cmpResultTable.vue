<script setup>
defineProps({
  resultats: Array
})
</script>

<template>
  <div class="mt-4">
    <h4 class="fw-bold text-dark mb-3">
      Resultados de la partida
    </h4>

    <div class="table-responsive rounded-4 shadow-sm border">
      <table class="table table-hover table-striped text-center align-middle mb-0">
        <thead class="table-dark">
          <tr>
            <th>Tecla</th>
            <th>Objectiu</th>
            <th>Temps real</th>
            <th>Error</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="r in resultats" :key="r.tecla">
            <td>
              <span class="badge text-bg-primary rounded-pill px-3 py-2">
                {{ r.tecla }}
              </span>
            </td>

            <td>{{ r.objectiu }} s</td>

            <td class="fw-semibold">
              {{ r.tempsReal.toFixed(2) }} s
            </td>

            <td>
              <span
                class="badge rounded-pill px-3 py-2"
                :class="Math.abs(r.error) < 0.1 ? 'text-bg-success' : 'text-bg-warning'"
              >
                {{ r.error > 0 ? '+' : '' }}{{ r.error.toFixed(2) }} s
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
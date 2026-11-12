<script setup>
import { useGameStore } from '@/stores/game'
import { computed } from 'vue'

const store = useGameStore()

const millorMarca = computed(() => {
  if (store.partides.length === 0) return null

  let millor = store.partides[0].errorTotal

  for (let partida of store.partides) {
    if (partida.errorTotal < millor) {
      millor = partida.errorTotal
    }
  }

  return millor
})
</script>

<template>
  <div class="card border-0 rounded-4">
    <div class="card-body p-3 p-md-4">

      <!-- CABECERA -->
      <div class="d-flex align-items-center gap-3 mb-4">
        <div class="bg-primary-subtle text-primary rounded-3 p-3">
          <i class="bi bi-trophy-fill fs-4"></i>
        </div>

        <div>
          <h4 class="fw-bold mb-1">Historial de partidas</h4>
          <p class="text-secondary small mb-0">
            Consulta tus resultados anteriores.
          </p>
        </div>
      </div>

      <!-- CLASSIFICACIÓ GLOBAL -->
      <div class="bg-body-tertiary border rounded-4 p-3 p-md-4 mb-4">

        <div class="d-flex align-items-center gap-3 mb-3">
          <div class="bg-success-subtle text-success rounded-3 p-3">
            <i class="bi bi-globe2 fs-4"></i>
          </div>

          <div>
            <h5 class="fw-bold mb-1">Clasificación global</h5>
            <p class="text-secondary small mb-0">
              Resultados de todos los jugadores.
            </p>
          </div>
        </div>

        <div v-if="store.classificacio.length === 0" class="text-center py-4">
          <i class="bi bi-bar-chart-line text-secondary fs-1"></i>
          <p class="text-secondary mt-2 mb-0">
            Todavía no hay resultados en la clasificación.
          </p>
        </div>

        <div v-else class="table-responsive">
          <table class="table table-hover align-middle mb-0">

            <thead class="table-light">
              <tr>
                <th class="py-3">Posición</th>
                <th class="py-3">Jugador</th>
                <th class="py-3 text-end">Error total</th>
              </tr>
            </thead>

            <tbody>
              <tr
                v-for="(resultat, index) in store.classificacio"
                :key="index"
              >
                <td>
                  <span
                    class="badge rounded-pill px-3 py-2"
                    :class="index === 0 ? 'text-bg-warning' : 'text-bg-secondary'"
                  >
                    #{{ index + 1 }}
                  </span>
                </td>

                <td class="fw-semibold">
                  {{ resultat.nom }}
                </td>

                <td class="text-end">
                  <span class="fw-bold text-success">
                    {{ resultat.errorTotal }} s
                  </span>
                </td>
              </tr>
            </tbody>

          </table>
        </div>
      </div>

      <!-- HISTORIAL LOCAL -->
      <div class="d-flex align-items-center gap-2 mb-3">
        <i class="bi bi-clock-history text-primary fs-4"></i>
        <h5 class="fw-bold mb-0">Mis partidas</h5>
      </div>

      <div v-if="store.partides.length === 0" class="text-center py-4">
        <i class="bi bi-controller text-secondary fs-1"></i>
        <p class="text-secondary mt-2 mb-0">
          Todavía no tienes partidas guardadas.
        </p>
      </div>

      <div v-else class="table-responsive">
        <table class="table table-hover align-middle mb-0">

          <thead class="table-light">
            <tr>
              <th class="py-3">Partida</th>
              <th class="py-3">Jugador</th>
              <th class="py-3 text-end">Error total</th>
            </tr>
          </thead>

          <tbody>
            <tr
              v-for="partida in store.partides"
              :key="partida.id"
            >
              <td>
                <span class="badge text-bg-primary rounded-pill px-3 py-2">
                  #{{ partida.id }}
                </span>
              </td>

              <td class="fw-semibold">
                {{ partida.nom }}
              </td>

              <td class="text-end">
                <span class="fw-bold text-primary">
                  {{ partida.errorTotal }} s
                </span>
              </td>
            </tr>
          </tbody>

        </table>

        <button
          @click="store.esborrarPartides()"
          class="btn btn-outline-danger rounded-3 mt-3"
        >
          <i class="bi bi-trash me-2"></i>
          Borrar historial
        </button>
      </div>

      <!-- MEJOR MARCA -->
      <div
        v-if="millorMarca !== null"
        class="d-flex align-items-center justify-content-between bg-warning-subtle border border-warning-subtle rounded-4 p-3 mt-4"
      >
        <div class="d-flex align-items-center gap-3">
          <div class="bg-warning text-dark rounded-3 p-3">
            <i class="bi bi-trophy-fill fs-3"></i>
          </div>

          <div>
            <h6 class="fw-bold mb-1">Mejor marca</h6>
            <p class="text-secondary small mb-0">
              Tu mejor resultado hasta ahora.
            </p>
          </div>
        </div>

        <div class="text-end">
          <h3 class="fw-bold text-dark mb-0">
            {{ millorMarca.toFixed(2) }} s
          </h3>
        </div>
      </div>

    </div>
  </div>
</template>
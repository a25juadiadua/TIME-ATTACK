<script setup>
import { useGameStore } from '@/stores/game'
import {  computed } from 'vue'

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

      <!-- TABLA -->
      <div class="table-responsive">
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
                <span class="fw-bold"
                  :class="partida.errorTotal <= 0 ? 'text-success' : 'text-primary'">
                  {{ partida.errorTotal }} s
                </span>
              </td>
            </tr>
          </tbody>

        </table>
        <button
  v-if="store.partides.length > 0"
  @click="store.esborrarPartides()"
  class="btn btn-outline-danger rounded-3 mt-3">
  <i class="bi bi-trash me-2"></i>
  Borrar historial
</button>

          <!-- MEJOR MARCA -->
        <div
          v-if="millorMarca !== null"
          class="d-flex align-items-center justify-content-between bg-warning-subtle border border-warning-subtle rounded-4 p-3 mt-4">

          <div class="d-flex align-items-center gap-3">
            <div class="bg-warning text-dark rounded-3 p-3">
              <i class="bi bi-trophy-fill fs-3"></i>
            </div>

            <div>
              <h6 class="fw-bold mb-1">Mejor marca</h6>
              <p class="text-secondary small mb-0">
                Tu mejor resultado hasta ahora
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
  </div>
</template>

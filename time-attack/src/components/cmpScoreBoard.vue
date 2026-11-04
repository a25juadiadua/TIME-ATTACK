<script setup>
import { useGameStore } from '@/stores/game'

const store = useGameStore()
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
      </div>

    </div>
  </div>
</template>

<script setup>
import cmpKeyPad from '@/components/cmpKeyPad.vue';
import { ref, onMounted, onUnmounted } from 'vue';
import cmpResultTable from '@/components/cmpResultTable.vue';
import { useGameStore } from '@/stores/game'
import cmpScoreBoard from '@/components/cmpScoreBoard.vue';

const store = useGameStore()
const nom = ref('')

const tecles = ['A', 'S', 'D', 'F']

const tempsObjectiu = {
  A: 2,
  S: 4,
  D: 5,
  F: 7
}

const jugant = ref(false)
const resultats = ref([])

let tempsInici = 0

function iniciarPartida(){
  tempsInici = performance.now();
  jugant.value = true;
  resultats.value = [];
  console.log("Se ha iniciado la partida")
}

function premuda(tecla){
  let ahora = performance.now()
  let repetido = false;
  let error;
  let teclaPresionada = tecles.includes(tecla);

  if(jugant.value && teclaPresionada){
    resultats.value.forEach(element => {
      if(element.tecla === tecla){
        console.log("Esta repetida la tecla")
        repetido = true;
      }
    });

    if(!repetido){
      let tiempoReal = (ahora - tempsInici) / 1000;
      error = tiempoReal - tempsObjectiu[tecla];

      console.log("Añadiendo el resultado")
      resultats.value.push({
        tecla: tecla,
        objectiu: tempsObjectiu[tecla],
        tempsReal: tiempoReal,
        error: error
      });
    }

    if (resultats.value.length === tecles.length) {
      jugant.value = false;

      const errorTotal = resultats.value.reduce(
        (total, r) => total + r.error, 0)

      store.guardarPartida(Number(errorTotal.toFixed(2)))
    }
  }
}

function comprobarTecla (event){
  if(event.repeat) return;

  let teclaPresionada = event.key.toUpperCase();

  if(tecles.includes(teclaPresionada)){
    premuda(teclaPresionada);
  }
}

function entrar() {
  if (nom.value.trim() === '') return
  store.guardarNom(nom.value.trim())
}

onMounted(() => {
  window.addEventListener('keydown', comprobarTecla);
});

onUnmounted(() => {
  window.removeEventListener('keydown', comprobarTecla);
});
</script>

<template>
  <main class="container-fluid min-vh-100 bg-body-tertiary p-4">

    <div class="container py-4">

      <!-- FORMULARIO -->
      <div class="min-vh-100 d-flex align-items-center justify-content-center"
        v-if="!store.nomJugador && !jugant">

        <form
          @submit.prevent="entrar"
          class="card border-0 shadow-lg rounded-4 p-4 p-md-5 text-center"
          style="max-width: 450px; width: 100%;">

          <div class="mb-3">
            <i class="bi bi-stopwatch-fill text-primary" style="font-size: 3.5rem;"></i>
          </div>

          <span class="badge text-bg-primary rounded-pill px-3 py-2 mb-3 mx-auto">
            TIME ATTACK
          </span>

          <h2 class="fw-bold mb-2">¡Bienvenido!</h2>

          <p class="text-secondary mb-4">
            Prepárate para poner a prueba tus reflejos.
          </p>

          <input
            v-model="nom"
            class="form-control form-control-lg rounded-3 text-center mb-3"
            placeholder="Introduce tu nombre"
          />

          <button
            type="submit"
            class="btn btn-primary btn-lg rounded-3 fw-semibold shadow-sm">
            Entrar al juego
            <i class="bi bi-arrow-right ms-2"></i>
          </button>

        </form>
      </div>

      <!-- HISTORIAL DE PARTIDAS -->
<div
  v-if="store.nomJugador && store.partides.length >= 1"
  class="card border-0 shadow-sm rounded-4 mb-4 mx-auto" style= "max-width: 650px;">

  <div class="card-body p-4">
    <div class="d-flex align-items-center gap-2 mb-3">
      <i class="bi bi-clock-history text-primary fs-4"></i>
      <h5 class="fw-bold mb-0">Historial de partidas</h5>
    </div>

    <cmpScoreBoard />
  </div>

</div>

      <!-- TARJETA DEL JUEGO -->
      <div
        v-if="store.nomJugador && resultats.length !== tecles.length"
        class="card border-0 shadow-lg rounded-4 mx-auto"
        style="max-width: 650px;">

        <div class="card-body text-center p-4 p-md-5">

          <div class="mb-3">
            <i class="bi bi-stopwatch-fill text-primary" style="font-size: 3rem;"></i>
          </div>

          <span class="badge text-bg-primary rounded-pill mb-3 px-3 py-2">
            TIME ATTACK
          </span>

          <h2 class="fw-bold text-dark mb-2">
            ¡Hola, {{ store.nomJugador }}!
          </h2>

          <h1 class="fw-bold text-dark mb-3">
            Pon a prueba tus reflejos
          </h1>

          <p class="text-secondary mb-4">
            Pulsa las teclas e intenta acertar el tiempo objetivo.
          </p>

          <!-- TECLADO -->
          <div class="bg-body-tertiary rounded-4 border p-4 mb-4">
            <cmpKeyPad
              :tecles="tecles"
              :bloquejat="!jugant"
              :premudes="resultats.map(r => r.tecla)"
              @premuda="premuda"
            />
          </div>

          <!-- ESTADO -->
          <div class="d-flex justify-content-center align-items-center gap-2 mb-4">

            <span
              class="rounded-circle"
              :class="jugant ? 'bg-success' : 'bg-secondary'"
              style="width: 10px; height: 10px;">
            </span>

            <p class="text-secondary small mb-0">
              {{ jugant ? '¡Partida en curso! Demuestra tus reflejos.' : '¿Preparado para empezar?' }}
            </p>

          </div>

          <!-- BOTÓN -->
          <button
            v-if="!jugant"
            class="btn btn-primary btn-lg rounded-3 px-5 fw-semibold shadow-sm"
            @click="iniciarPartida()">

            <i class="bi bi-play-fill me-2"></i>
            Iniciar partida

          </button>

        </div>
      </div>

      <!-- RESULTADOS -->
      <div
        v-if="!jugant && resultats.length === tecles.length"
        class="card border-0 shadow-lg rounded-4 mx-auto"
        style="max-width: 750px;">

        <div class="card-body p-4 p-md-5">

          <div class="text-center mb-4">

            <div class="mb-3">
              <i class="bi bi-trophy-fill text-warning" style="font-size: 3.5rem;"></i>
            </div>

            <span class="badge text-bg-success rounded-pill px-3 py-2 mb-3">
              PARTIDA FINALIZADA
            </span>

            <h2 class="fw-bold text-dark">
              ¡Buen trabajo!
            </h2>

            <p class="text-secondary mb-0">
              Estos son los resultados de tu partida.
            </p>

          </div>

          <div class="bg-body-tertiary rounded-4 border p-3 p-md-4">
            <cmpResultTable :resultats="resultats" />
          </div>

          <div class="text-center mt-4">
            <button
              class="btn btn-primary btn-lg rounded-3 px-5 fw-semibold shadow-sm"
              @click="iniciarPartida()">

              <i class="bi bi-arrow-repeat me-2"></i>
              Volver a jugar

            </button>
          </div>

        </div>
      </div>

    </div>
  </main>
</template>

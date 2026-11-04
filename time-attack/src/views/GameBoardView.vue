<script setup>
import cmpKeyPad from '@/components/cmpKeyPad.vue';
import { ref, onMounted, onUnmounted } from 'vue';
import cmpResultTable from '@/components/cmpResultTable.vue';

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

onMounted(() => {
  window.addEventListener('keydown', comprobarTecla);
});

onUnmounted(() => {
  window.removeEventListener('keydown', comprobarTecla);
});
</script>

<template>
  <main class="container-fluid min-vh-100 d-flex align-items-center justify-content-center bg-body-tertiary p-4">

    <!-- TARJETA DEL JUEGO -->
    <div
      v-if="resultats.length != tecles.length"
      class="card border-0 shadow-lg rounded-4 w-100"
      style="max-width: 650px;"
    >
      <div class="card-body text-center p-4 p-md-5">

        <span class="badge text-bg-primary rounded-pill mb-3 px-3 py-2">
          TIME ATTACK
        </span>

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
        <p class="text-secondary small mb-3">
          {{ jugant ? '¡Partida en curso! Demuestra tus reflejos.' : '¿Preparado para empezar?' }}
        </p>

        <!-- BOTÓN -->
        <button
  v-if="!jugant"
  class="btn btn-primary btn-lg rounded-3 px-5 fw-semibold shadow-sm"
  @click="iniciarPartida()"
>
  <i class="bi bi-play-fill me-2"></i>
  Iniciar partida
</button>

      </div>
    </div>

    <!-- RESULTADOS -->
    <div
      v-if="!jugant && resultats.length === tecles.length"
      class="card border-0 shadow-lg rounded-4 w-100"
      style="max-width: 750px;"
    >
      <div class="card-body p-4 p-md-5">

        <div class="text-center mb-4">
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

        <cmpResultTable :resultats="resultats" />

        <div class="text-center mt-4">
          <button
            class="btn btn-primary btn-lg rounded-3 px-5 fw-semibold shadow-sm"
            @click="iniciarPartida()"
          >
            <i class="bi bi-arrow-repeat me-2"></i>
            Volver a jugar
          </button>
        </div>

      </div>
    </div>

  </main>
</template>
<script setup>
import cmpKeyPad from '@/components/cmpKeyPad.vue';
import { ref } from 'vue';

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


  if(jugant.value){

    resultats.forEach(element => {

      if(element === tecla){
        console.log("Esta repetida la tecla")
        repetido = true;
      }

    });

    if(!repetido){

      error = ahora - tempsInici;


      console.log("Añadiendo el resultado")
        resultats.value.push({
          tecla: tecla,
          objectiu: tempsObjectiu[tecla],
          tempsReal: ahora,
          error: error
        });
      }

  }

}
</script>

<template>
  <main class="container-fluid min-vh-100 d-flex align-items-center justify-content-center bg-body-tertiary p-4">
    <div class="card border-0 shadow-lg rounded-4 w-100" style="max-width: 650px;">
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

        <div class="bg-body-tertiary rounded-4 border p-3 p-md-4">
          <cmpKeyPad   
            :tecles="tecles"
            :bloquejat="!jugant"
            @premuda="premuda"
          />
        </div>

        <p class="text-secondary small mt-4 mb-0">
          ¿Preparado para empezar?
        </p>

        <button @click="iniciarPartida()">Iniciar partida</button>
      </div>
    </div>
  </main>
</template>
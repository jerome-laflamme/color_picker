<script setup lang="ts">
import { ref } from "vue";

const nbOfSqr = ref(0);
let isEmpty = true;
let colorExists = ref(false);

interface Color {
  r: number;
  g: number;
  b: number;
}

const colorWatch = ref<Color>({ r: 10, g: 10, b: 10 });
const color = ref('rgb(0,0,0)');
const savedColors = ref([] as string[]);

function updateColor() {
  color.value = `rgb(${colorWatch.value.r},${colorWatch.value.g},${colorWatch.value.b})`;
}  

function getColor(colorToCopy : string) {
  color.value = colorToCopy;
}

function saveColor() {  
  isEmpty = false;
  savedColors.value.find((savedColor) => savedColor === color.value) ? colorExists.value = true : colorExists.value = false;
  if (!colorExists.value) {
    savedColors.value.push(color.value);
    colorWatch.value = { r: 10, g: 10, b: 10 };
    color.value = 'rgb(0,0,0)';
    nbOfSqr.value++
  }
}

function reset() {
  savedColors.value = [];
  nbOfSqr.value = 0;
}
</script>

<template>
  <section>
    <h2>Nombre de carrés : {{nbOfSqr}}</h2>
    <div>
      <div class="square" :style="{backgroundColor: color}"></div>

      <div class="sliders">
        <div class="slider">
        <label for="red">R</label>
        <input type="range" id="red" v-model="colorWatch.r" min="0" max="255" @input="updateColor()">
        </div>
        <div class="slider">
          <label for="green">G</label>
          <input type="range" id="green" v-model="colorWatch.g" min="0" max="255" @input="updateColor()">
        </div>
        <div class="slider">
          <label for="blue">B</label>
          <input type="range" id="blue" v-model="colorWatch.b" min="0" max="255" @input="updateColor()">
        </div>
      </div>
      
      <div class="buttons">
        <button @click="saveColor()">Sauvegarder</button>
        <button @click="reset()">Reset</button>
      </div>
      <div v-if="isEmpty">
        <h3>Ajouter des couleurs!</h3>
      </div>
      <div v-else id="colorContainer" >
        <!-- colors are saved here -->
        <div v-for="savedColor in savedColors" :style="{backgroundColor: savedColor}" class="prevsquare" @click="getColor(($event.target as HTMLElement).style.backgroundColor)">
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped></style>
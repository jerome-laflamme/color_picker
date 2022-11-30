<script setup lang="ts">
import type { Color } from "csstype";
import { ref } from "vue";

const nbOfSqr = ref(0);

interface Color {
  r: number;
  g: number;
  b: number;
}

const colorWatch = ref<Color>({ r: 10, g: 10, b: 10 });

const color = ref('rgb(0,0,0)');
const savedColors = ref([] as Color[]);

function updateColor() {
  color.value = `rgb(${red.value},${green.value},${blue.value})`;
}  

function getColor(colorToCopy : string) {
  console.log(colorToCopy);
  // color.value = colorToCopy;
}

function saveColor() {
  savedColors.value.push(color.value);
  nbOfSqr.value++
}

function reset() {
  savedColors. = "";
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
        <input type="range" id="red" v-model="red" min="0" max="255" @input="updateColor()">
        </div>
        <div class="slider">
          <label for="green">G</label>
          <input type="range" id="green" v-model="green" min="0" max="255" @input="updateColor()">
        </div>
        <div class="slider">
          <label for="blue">B</label>
          <input type="range" id="blue" v-model="blue" min="0" max="255" @input="updateColor()">
        </div>
      </div>
      
      <div class="buttons">
        <button @click="saveColor()">Sauvegarder</button>
        <button @click="reset()">Reset</button>
      </div>
      
      <div id="colorContainer" >
        <div v-for="savedColor in savedColors" :style="{backgroundColor: color}">

        </div>
        <!-- colors are saved here -->
      </div>
    </div>
  </section>
</template>

<style scoped>

</style>

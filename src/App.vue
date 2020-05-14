<template>
<div id="application">
  <head>
    <title>Sorting Visualiser</title>
  </head>
  <body>
    <div id="app">
      <div id="headerBar">
        <div id="pageTitle">Sorting Visualiser</div>
        <div id="controls">
          <div id="Dropdown">
            <select v-model="sortingMode" v-if="!sorting">
              <option value="bubbleSort">Bubble Sort</option>
              <option value="insertionSort">Insertion Sort</option>
              <option value="mergeSort">Merge Sort</option>
            </select>
          </div>
          <div class="sliderContainer">
            <div class="sliderText">Adjust Sorting Speed</div>
            <input class="slider" type="range" min="0" max="300" value="50" v-model="sortSpeed" />
          </div>
          <div class="sliderContainer" v-if="!sorting">
            <div class="sliderText">Adjust Array Size</div>
            <input class="slider" type="range" min="1" max="100" value="50" v-model="arraySize" />
          </div>
          <button id="scramble" @click="scramble" v-if="!sorting">Scramble</button>
          <button id="startSorting" @click="sort" v-if="!sorting && !sorted">Start Sorting</button>
        </div>
      </div>
      <div id="mainBody" :key="key">
        <div
          class="valueBlockPair"
          v-for="(value, index) in values"
          :key="value.id"
          :style="{ width: blockWidth }"
        >
          <div
            class="block"
            :class="[
                { green: values[index].sorted },
                { purple: values[index].active },
              ]"
            :style="{ height: values[index].id + '%' }"
          ></div>
          <!--<div class="value">{{ value }}</div> -->
        </div>
      </div>
    </div>
  </body>
</div>
</template>

<script>
export default {
  name: "App",
  
};
</script>

<style>
body {
  margin: 0px auto;
  width: 100%;
  font-family: "H", "Trebuchet MS", Helvetica;
  background-color: #f2f2f2;
  color: #cfcfcf;
}

button {
  background-color: #f2f2f2;
  border: 1px solid gray;
  border-radius: 10px;
  border: none;
  margin: 20px;
  padding: 10px;
  font-size: 18px;
  opacity: 0.8;
  cursor: pointer;
}

button:hover {
  opacity: 1;
}

#app {
  display: flex;
  flex-direction: column;
}

#controls {
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 100%;
}

#headerBar {
  width: 100%;
  height: 10%;
  background-color: #4a4a4a;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-end;
}

#pageTitle {
  font-size: 220%;
  margin-right: auto;
  padding: 20px;
  float: left;
}

#mainBody {
  position: fixed;
  bottom: 0;
  height: 85%;
  margin: 5% 10% 0 10%;
  width: 80%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-content: flex-end;
  align-items: flex-end;
}

select {
  margin: 20px;
  height: 30px;
  width: 160px;
  border-radius: 7px;
  border: none;
  background-color: #f2f2f2;
  opacity: 0.8;
}

select:hover {
  opacity: 1;
}

.sliderContainer {
  text-align: center;
  margin: 20px;
}

.slider {
  -webkit-appearance: none;
  appearance: none;
  height: 15px;
  width: 160px;
  background-color: #f2f2f2;
  border-radius: 7px;
  margin: 5px;
  opacity: 0.8;
}

input::-webkit-slider-thumb {
  -webkit-appearance: none;
  height: 15px;
  width: 15px;
  border-radius: 7px;
  border: none;
  appearance: none;
  background: #4caf50;
  cursor: pointer;
}

input::-moz-range-thumb {
  background: #4caf50;
  width: 15px;
  border: none;
  height: 15px;
  cursor: pointer;
}

.slider:hover {
  opacity: 1;
}

.valueBlockPair {
  height: 100%;
  width: 5px;
  margin: 1px;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

.block {
  height: 100%;
  width: 90%;
  background-color: #c70d00;
}

.value {
  text-align: center;
  width: 20px;
  margin: 2px;
}

.green {
  background-color: green;
}

.purple {
  background-color: purple;
}
</style>
<template>
  <div id="app">
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
              <input
                class="slider"
                type="range"
                min="0"
                max="300"
                value="50"
                v-model="sortSpeed"
              />
            </div>
            <div class="sliderContainer" v-if="!sorting">
              <div class="sliderText">Adjust Array Size</div>
              <input
                class="slider"
                type="range"
                min="1"
                max="100"
                value="50"
                v-model="arraySize"
              />
            </div>
            <button id="scramble" @click="scramble" v-if="!sorting">
              Scramble
            </button>
            <button id="startSorting" @click="sort" v-if="!sorting && !sorted">
              Start Sorting
            </button>
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
  components: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

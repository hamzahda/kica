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
  data: function() {
    return {
      sortingMode: "bubbleSort",
      arraySize: 50,
      sortSpeed: 150,
      values: [],
      sorting: false,
      sorted: false
    };
  },
  methods: {
  scramble: function() {
      this.values = [];
      this.sorting = false;
      this.sorted = false;
      for (let i = 0; i < this.arraySize; i++) {
        /*Looks strange but I use an object here so rerendering occurs for individual blocks instead
                  Of having the whole app rerender on every bubblesort iteration, also allows me to add colour information */
        let obj = {
          id: Math.random() * 100,
          sorted: false,
          active: false
        };
        this.values.push(obj);
      }
    },
    async sort() {
      this.sorting = true;
      if (this.sortingMode === "bubbleSort") {
        await this.bubbleSort();
      } else if (this.sortingMode === "insertionSort") {
        await this.insertionSort();
      } else {
        let arr = [];
        for (let i = 0; i < this.arraySize; i++) {
          arr[i] = this.values[i].id;




        }
        // eslint-disable-next-line no-unused-vars
        let sortedArray = await this.mergeSort(arr, 0);
        for (let i = 0; i < this.arraySize; i++) {
          this.values[i].sorted = true;
        }
      }
      //Resetting state variable so you can sort again
      this.sorting = false;
      this.sorted = true;
    },
    //Simple bubble sort algorithm

    async bubbleSort() {
      for (let i = 0; i < this.arraySize; i++) {
        for (let j = 0; j < this.arraySize - i - 1; j++) {
          //Changes the current active bar to purple
          this.values[j].active = true;
          await this.swapValues(j, j + 1);
          //change the active bars to false
          this.values[j].active = false;
          this.values[j + 1].active = false;
        }
        //Changes bar colour to green
        this.values[this.arraySize - i - 1].sorted = true;
      }
    },
    //Split into separate function here to deal with the async problems that arise with having setTimeout in a loop
    swapValues: function(j, active) {
      if (this.values[j].id > this.values[j + 1].id) {
        //If a bar is being switched, colour it purple
        this.values[active].active = true;
        //Switch the two values
        let tmp = this.values[j].id;
        this.values[j].id = this.values[j + 1].id;
        this.values[j + 1].id = tmp;
      }
      // wait for sortSpeed amt of time
      return new Promise(resolve => setTimeout(resolve, 300 - this.sortSpeed));
    },
  computed: {
    blockWidth: function() {
      return 100 / this.arraySize + "%";
    }
  },
  created() {
    this.scramble();
  },
  watch: {
    arraySize: function() {
      this.scramble();
    }
  }
  }
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
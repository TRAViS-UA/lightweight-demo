<template>
  <button @click="onStartStop">{{ isActive ? 'Stop' : 'Start' }}</button>
  <button :disabled="!isActive" @click="() => onSetData('all')">setData() all data</button>
  <button :disabled="!isActive" @click="() => onSetData('lastonly')">setData() last 1000</button>
  <button :disabled="!isActive" @click="() => onSetData('clear')">Clear data</button>
  <span> data count ({{ chartDataCounter }})</span>
  <div class="chart-container">
    <tvChart ref="lwChart" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { IChartValue } from './components/interface';
import tvChart from './components/tvChart.vue';
const lwChart = ref();
let currentValue: IChartValue = { time: Date.now(), value: 2000 }
const aggrTime = 1;
const chartData: IChartValue[] = [];
const chartDataCounter = ref(0);
const speedTime = 10;
const isActive = ref(false);
let interval = 0;

function onSetData(type: 'all' | 'lastonly' | 'clear') {
  switch (type) {
    case 'all':
      lwChart.value.setDataTVChart(chartData);
      break;
    case 'lastonly':
      chartData.splice(0, chartData.length - 1000)
      lwChart.value.setDataTVChart(chartData);
      break;
    case 'clear':
      chartData.length = 0;
      lwChart.value.setDataTVChart([]);
      break;
  }
}

function getNewData(oldValue: IChartValue) {
  const newValue = oldValue.value + Math.round(Math.random() * 16 - 8);
  return { time: oldValue.time + aggrTime, value: newValue };
}

function onStartStop() {
  isActive.value = !isActive.value;
  if (isActive.value) {
    lwChart.value.resetTVChart();
    interval = setInterval(() => {
      currentValue = getNewData(currentValue);
      chartDataCounter.value = chartData.push(currentValue);
      lwChart.value.updateTVChart(currentValue);
    }, speedTime);
  } else {
    clearInterval(interval);
    isActive.value = false;
  }
}

</script>

<style scoped>
.chart-container {
  height: 80vh;
  width: 100vw;
}
</style>
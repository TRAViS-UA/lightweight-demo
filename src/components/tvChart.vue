<template>
    <div class="lw-chart" ref="chartContainer"></div>
</template>

<script setup lang="ts">
import { createChart, type IChartApi } from 'lightweight-charts';
import { onMounted, onUnmounted, ref } from 'vue';
import { IChartValue } from './interface';

const chartContainer = ref();
let chart: IChartApi;
let series: any;
let series2: any;
let series3: any;

defineExpose({ createTVChart, updateTVChart, resetTVChart, setDataTVChart });

function resetTVChart() {
    series.setData([]);
    series2.setData([]);
    series3.setData([]);
}

function createTVChart() {
    series = chart.addLineSeries();
    series2 = chart.addLineSeries();
    series3 = chart.addLineSeries();
}

function updateTVChart(data: IChartValue) {
    // console.log('updateTVChart', data);
    series.update(data);
    series2.update({ time: data.time, value: data.value + 10 });
    series3.update({ time: data.time, value: data.value - 10 });
}
function setDataTVChart(data: IChartValue[]) {
    series.setData(data);
    series2.setData(data);
    series3.setData(data);
}

onMounted(() => {
    console.log('Chart is created');
    chart = createChart(chartContainer.value);
    createTVChart();
});

onUnmounted(() => {
    if (chart) {
        chart.remove();
    }
});



</script>

<style scoped>
.lw-chart {
    height: 100%;
}
</style>
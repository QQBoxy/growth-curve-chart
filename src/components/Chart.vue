<script setup lang="ts">
import { ref, onMounted } from 'vue';
import * as echarts from 'echarts';
import growthStandardChart from '../assets/GrowthStandardChart.json';


const percentile = ['3rd', '15th', '25th', '50th', '75th', '85th', '97th'] as const;

type EChartsOption = echarts.EChartsOption;

const chartDom = ref<HTMLElement | null>(null);
const myChart = ref<echarts.ECharts | null>(null);

onMounted(() => {
  if (!chartDom.value) {
    return;
  }
  myChart.value = echarts.init(chartDom.value);

  const series: EChartsOption['series'] = [];
  percentile.forEach((percentile) => {
    series.push({
      data: growthStandardChart.girl.height[percentile],
      type: 'line',
      smooth: true,
      name: percentile,
    });
  });
  // series.push({
  //   type: 'scatter',
  //   data: [
  //     ['1', 74.5]
  //   ],
  //   color: '#ff0000',
  // });

  const option: EChartsOption = {
    title: {
      text: '生長曲線圖'
    },
    tooltip: {
      trigger: 'axis'
    },
    xAxis: {
      type: 'category',
      boundaryGap: false,
      data: ['0', '0.5', '1', '1.5', '2', '2.5', '3', '3.5', '4', '4.5', '5', '5.5', '6', '6.5', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17'],
      axisLabel: {
        formatter: '{value} 歲'
      }
    },
    yAxis: {
      type: 'value',
      axisLabel: {
        formatter: '{value} 公分'
      }
    },
    series: [...series],
  };

  option && myChart.value.setOption(option);
});

</script>

<template>
  <div ref="chartDom" class="chart-container"></div>
</template>

<style scoped>
.chart-container {
  width: 100%;
  height: 50vw;
}
</style>

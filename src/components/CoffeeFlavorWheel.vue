<script setup lang="ts">
import * as echarts from 'echarts';
import {onMounted, onUnmounted} from "vue";
import flavor from "./coffee-flavor.json"


function processJSON(data:any) {
  return data.map((item: any) => {
    const processedItem:any = {
      name: item.label,
      itemStyle: item.itemStyle
    };
    if (item.children) {
      processedItem.children = processJSON(item.children);
    } else {
      processedItem.value = 1;
    }
    return processedItem;
  });
}


const option: any =  {
  series: [
    {
      type: 'sunburst',
      radius: [0, '95%'],
      sort: undefined,
      emphasis: {
        focus: 'ancestor'
      },
      animationDurationUpdate: 1000,
      data: processJSON(flavor),
      universalTransition: true,
      itemStyle: {
        borderWidth: 1,
        borderColor: 'rgba(255,255,255,.5)'
      },
      label: {
        show: true
      }
    }
  ]
}

let chart:any = null

function resizeChart() {
  chart && chart.resize();
}
onMounted(() => {
  // 获取dom，断言HTMLElement类型，否则会报错
  const chartEle: HTMLElement = document.getElementById('coffee-flavor') as HTMLElement;
  chart = echarts.init(chartEle);
  option && chart.setOption(option);
  // resize
  window.addEventListener('resize', resizeChart);
})

onUnmounted(() => {
  chart && chart.dispose();
  window.removeEventListener('resize', resizeChart);
})
</script>
<template>
  <div id="coffee-flavor" style="width: 100%;height: 100%;"></div>
</template>


<style scoped>
#coffee-flavor {
  margin: 0 auto;
}
</style>

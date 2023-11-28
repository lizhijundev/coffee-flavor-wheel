<script setup lang="ts">
import * as echarts from 'echarts';
import { ref, onMounted } from "vue";
import {EChartsOption, init} from "echarts";
import flavor from "./flavor.json"

const data: any = {
  name: "Coffee Flavor",
  value: "",
  children: flavor

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
      nodeClick: undefined,
      data: data.children,
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


onMounted(() => {

  // 获取dom，断言HTMLElement类型，否则会报错
  const chartEle: HTMLElement = document.getElementById('coffee-flavor') as HTMLElement;
  const chart = init(chartEle);

  option && chart.setOption(option);
  console.log(chartEle)



})
</script>
<template>
  <div>hello</div>

  <div id="coffee-flavor" style="width: 800px; height: 100vh;"></div>
</template>


<style scoped>
#coffee-flavor {
  margin: 0 auto;
}
</style>
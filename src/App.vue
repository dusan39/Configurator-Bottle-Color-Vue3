<template>
  
  <div class="title-container">
    <h1>Dna Information Analytics</h1>
  </div>

  <div class="container">
    <div class="chart-container">
      <Chart v-show="selectedChart === 0" id="myChartSubscriptions"></Chart>
      <Chart v-show="selectedChart === 1" id="myChartImpressions"></Chart>
      <Chart v-show="selectedChart === 2" id="myChartClicks"></Chart>
      <Chart v-show="selectedChart === 3" id="myChartAvgTime"></Chart>
    </div>
    
    <div class="controller-container">
      <Controller data-type="subscriptions" @click="showChart(0)"></Controller>
      <Controller data-type="impressions" @click="showChart(1)"></Controller>
      <Controller data-type="clicks" @click="showChart(2)"></Controller>
      <Controller data-type="avgTime" @click="showChart(3)"></Controller>
    </div>   
  </div>
  
</template>

<script setup>

  import Chart from './components/Chart.vue'
  import Controller from './components/Controller.vue';

  import { watch, ref } from 'vue' 

  let selectedChart = ref(0)

  const showChart = (chartIndex) => {
    selectedChart.value = chartIndex
  }

  watch(() => selectedChart.value, () => {}, { deep: true });
  
</script>

<style scoped lang="scss">
  .title-container{
    display: flex;
    justify-content: center;
    color: white;
    background-color: #146C94;
    margin-bottom: 2%;

    h1{
      padding: 20px 40px;
      text-align: center;
      border-radius: 20px;
      background-color: #19A7CE;
      border: 2px solid #F6F1F1;
    }
  }

  .container{
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;

    .chart-container{
      width: 50%;
      padding: 3% 4%;
      border: solid 2px #F6F1F1;
      border-radius: 20px;
    }

    .controller-container{
      display: flex;
      flex-direction: row;
    }
  }

    @media (max-width: 850px){
      .title-container{

        h1{
          font-size: 18px;
        }
      }

      .container{
        .chart-container{
          width: 500px !important;
        }

        .controller-container{
          display: grid !important;
          column-gap: 30px;
          row-gap: 30px;
          grid-template-columns: 150px 150px;
        }
      }
    }

    @media (max-width: 550px){
      .container{
        .chart-container{
          width: 350px !important;
        }

        .controller-container{
          column-gap: 5px;
          row-gap: 5px;
          grid-template-columns: 140px 140px;
        }
      }
    }

    @media (max-width: 410px){
      .container{
        .chart-container{
          width: 290px !important;
        }  
      }
    }

</style>
<template>

  <canvas></canvas>
    
</template>

<script> 

  import { getData, getSubscriptionsData, getImpressionsData, getClicksData, getAvgTimeData } from '../api';
  import Chart from 'chart.js/auto';

  async function createCharts() {
    const response = await getData()

    const subscriptionsDataAPI= await getSubscriptionsData(response)
    const impressionsDataAPI = await getImpressionsData(response)
    const clicksDataAPI = await getClicksData(response)
    const avgTimeDataAPI = await getAvgTimeData(response)

    const { subscriptionsLabels, subscriptionsData } = subscriptionsDataAPI
    const { impressionsLabels, impressionsData } = impressionsDataAPI
    const { clicksLabels, clicksData } = clicksDataAPI
    const { avgTimeLabels, avgTimeData } = avgTimeDataAPI

    createChartSubscriptions(subscriptionsLabels, subscriptionsData)
    createChartImpressions(impressionsLabels, impressionsData)
    createChartClicks(clicksLabels, clicksData)
    createChartAvgTime(avgTimeLabels, avgTimeData)
  }

  async function createChartSubscriptions(labels, data) {
    const config = createConfig(labels, data, 'Subscriptions');
    const myChartSubscriptions = new Chart(document.getElementById('myChartSubscriptions'), config);
  }

  async function createChartImpressions(labels, data) {
    const config = createConfig(labels, data, 'Impressions');
    const myChartImpressions = new Chart(document.getElementById('myChartImpressions'), config);
  }

  async function createChartClicks(labels, data) {
    const config = createConfig(labels, data, 'Clicks');
    const myChartClicks = new Chart(document.getElementById('myChartClicks'), config);
  }

  async function createChartAvgTime(labels, data) {
    const config = createConfig(labels, data, 'Avg Time');
    const myChartAvgTime = new Chart(document.getElementById('myChartAvgTime'), config);
  }

  function createConfig(labels, data, label) {
    return {
      type: 'line',
      data: {
        labels: labels,
        datasets: [{
          label: label,
          backgroundColor: '#19A7CE',
          borderColor: '#19A7CE',
          data: data,
        }]
      },
      options: {}
    };
  }

  window.addEventListener('DOMContentLoaded', () => {
    createCharts();
  });

</script>

<style lang="scss">

</style>
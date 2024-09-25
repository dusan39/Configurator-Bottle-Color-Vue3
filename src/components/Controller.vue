<template>
      
  <div class="chart-type-container">
    <div class="chart-button-container">
      <button>{{ type }}</button>
      <h4>Total: {{ total }}</h4>
    </div>
  </div>

</template>
 
<script setup>

  import { defineProps, reactive, computed, onMounted } from 'vue';
  import { getData } from '../api';

  const props = defineProps({
    dataType: String
  });

  const data = reactive({
    subscriptionsTotal: 0,
    impressionsTotal: 0,
    clicksTotal: 0,
    avgTimeTotal: 0,
  });

  async function getTotalData(){
    const response = await getData();
    data.subscriptionsTotal = response.data.subscriptions.total;
    data.impressionsTotal = response.data.impressions.total
    data.clicksTotal = response.data.clicks.total;
    data.avgTimeTotal = response.data.avgTime.total;
  }

  function startIncrementingImpressions() {
    setInterval(() => {
      data.impressionsTotal += 5;
    }, 2000);
  }

  const total = computed(() => {
    switch (props.dataType) {
      case 'subscriptions':
        return data.subscriptionsTotal;
      case 'impressions':
        return data.impressionsTotal;
      case 'clicks':
        return data.clicksTotal;
      case 'avgTime':
        return data.avgTimeTotal;
      default:
        return '';
    }
  });

  const type = computed(() => {
    switch (props.dataType){
      case 'subscriptions':
        return 'Subscriptions';
      case 'impressions':
        return 'Impressions';
      case 'clicks':
        return 'Clicks';
      case 'avgTime':
        return 'Avg Time';
      default:
        return '';
    }
  })

  onMounted(() => {
    getTotalData(),
    startIncrementingImpressions()
  });

</script>

<style lang="scss">

  .chart-type-container{
    margin-top: 3%;

    .chart-button-container{
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      align-items: center;
      
      background-color: #146C94;
      border: 2px solid #F6F1F1;
      border-radius: 20px;
      padding: 1rem 1rem;
      margin: 5px;

      button{

        cursor: pointer;
        padding: 8px 16px;
        border-radius: 10px;
        background-color: #AFD3E2;
        border: 2px solid #F6F1F1;
        transition: all 0.3s;

        &:hover{
          border: 2px solid #AFD3E2;
          background-color: #F6F1F1;
        }
      }

      h4{
        color: white;
      }
    }
  }

</style>
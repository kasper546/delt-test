<template>
  <div class="table_top">
      <div class="indicator">
        <p class="indicator_p_first">Показатель</p>
        <p class="indicator_p">Выручка, руб</p>
      </div>
      <div class="day_top">
        <p class="day_p_first">Текущий день</p>
        <p class="day_p">500 521</p>
      </div>
      <div class="yesterday_top">
        <p class="yesterday_p_first">Вчера</p>
        <p class="yesterday_p yesterday_p_green">480 521 <span class="green_percent">4%</span> </p>
      </div>
      <div class="day_of_week_top">
        <p class="day_of_week_p_first">Этот день недели</p>
        <p class="day_of_week_p p_red">480 521</p>
      </div>
  </div>
  <div id="graph">
    <canvas ref="canvas"></canvas>
    <ul>
      <li v-for="(item, index) in rows"
       :key="index"
        @click="updateChart(item)">
        <p class="indicator_p">{{ item.title }}</p> <p class="day_p">{{ item.value1 }}</p> <p class="yesterday_p" :style="{ 
          backgroundColor: index === 3 || index === 4 ? '#ecf7e7' : 
                 index === 5 || index === 7 || index === 8 ? '#fee6e6' : 
                 '#f5f5f5' 
        }">{{ item.value2 }} <span :style="{ color: index === 5 || index === 7 || index === 8 ? '#ff6968' : ' #6F9628' }">{{ item.percent }}</span></p>
         <p class="day_of_week_p" :style="{ backgroundColor: index === 0 || index === 1 || index === 2 || index === 7 ? '#f5f5f5' : ' #ecf7e7'}">{{ item.value3 }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { Chart, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement, LineController } from 'chart.js';

// Регистрация компонентов Chart.js
Chart.register(Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement, LineController);

const canvas = ref(null);
let chartInstance = null;

const rows = [
  { title: 'Наличные', value1: 300000, value2: 300000, percent: "0%", value3: 300000 },
  { title: 'Безналичный расчет', value1: 100000, value2: 100000, percent: "0%", value3: 100000 },
  { title: 'Кредитные карты', value1: 100521, value2: 100521, percent: "0%", value3: 100521 },
  { title: 'Средний чек, руб', value1: 1300, value2: 1300, percent: "44%", value3: 900 },
  { title: 'Средний гость, руб', value1: 1200, value2: 1200, percent: "50%", value3: 800 },
  { title: 'Удаления из чека (после оплаты), руб', value1: 1000, value2: 1000, percent: "-9%", value3: 900 },
  { title: 'Удаления из чека (до оплаты), руб', value1: 1300, value2: 1300, percent: "0%", value3: 900 },
  { title: 'Количество чеков', value1: 34, value2: 36, percent: "-6%", value3: 34 },
  { title: 'Количество гостей', value1: 34, value2: 36, percent: "-6%", value3: 32 }
];

const chartOptions = {
  responsive: true,
  plugins: {
    legend: {
      position: 'top'
    },
  }
};

const updateChart = (item) => {
  const chartData = {
    labels: ['', '', ''],
    datasets: [
      {
        label: item.title,
        backgroundColor: 'rgba(66, 165, 245, 0.2)',
        borderColor: '#42A5F5',
        data: [item.value1, item.value2, item.value3],
        fill: true
      }
    ]
  };

  if (chartInstance) {
    chartInstance.data = chartData;
    chartInstance.update();
  }
};

onMounted(() => {
  const initialData = {
    labels: ['', '', ''],
    datasets: [
      {
        label: '',
        backgroundColor: 'rgba(66, 165, 245, 0.2)',
        borderColor: '#42A5F5',
        data: [rows[0].value1, rows[0].value2, rows[0].value3],
        fill: true
      }
    ]
  };

  if (canvas.value) {
    chartInstance = new Chart(canvas.value, {
      type: 'line',
      data: initialData,
      options: chartOptions
    });
  }
});
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
.table_top{
  display: grid;
  grid-template-columns: 200px 150px 100px 200px;  
  gap: 10px;      
  text-align: center;            
}

.indicator_p {
  background-color: #f5f5f5;
  height: 40px;
  text-align: left;
}


.day_p {
  background-color: #edf8ff;
  height: 40px;
  display: flex;
  justify-content: right;
  padding-right: 30px;
}


.yesterday_p {
  background-color: #f5f5f5;
  height: 40px;
  display: flex;
  justify-content: space-around;
}


.day_of_week_p {
  background-color: #f5f5f5;
  height: 40px;
  justify-content: right;
  padding-right: 30px;
}





.indicator_p_first {
  background-color: #f5f5f5;
  height: 40px;
  justify-content: center;

}


.day_p_first {
  background-color: #edf8ff;
  height: 40px;
  text-align: center;
  justify-content: center;
}


.yesterday_p_first {
  background-color: #f5f5f5;
  height: 40px;
  text-align: center;
  justify-content: center;
}


.day_of_week_p_first {
  background-color: #f5f5f5;
  height: 40px;
  text-align: center;
  justify-content: center;
}

.green_percent {
  color: #6F9628;
}
.red_percent {
  color: #ff6968;
}

li {
  gap: 10px;
  display: grid;
  grid-template-columns: 200px 150px 100px 200px;  
}
p {
  margin-top: 5px;
  display: flex;
  align-items: center;
}
.p_green {
  background-color: #ecf7e7;
}
.p_red {
  background-color: #fee6e6;
}
#graph {
  width: 683px;
}
</style>

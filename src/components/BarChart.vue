<template>
    <div>
      <Bar :chart-data="chartData" :options="chartOptions" />
    </div>
  </template>

  <script setup>
  import { ref } from 'vue';
  import { Bar } from 'vue3-chartjs';
  import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    BarElement,
    CategoryScale,
    LinearScale,
  } from 'chart.js';

  // Регистрация компонентов Chart.js
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

  // Данные графика
  const chartData = ref({
    labels: ['January', 'February', 'March', 'April', 'May'],
    datasets: [
      {
        label: 'Sales',
        backgroundColor: '#42A5F5',
        data: [40, 20, 12, 39, 10],
      },
    ],
  });

  // Опции графика
  const chartOptions = ref({
    responsive: true,
    maintainAspectRatio: false,
  });
  </script>

  <style scoped>
  /* Добавьте свои стили, если нужно */
  </style>
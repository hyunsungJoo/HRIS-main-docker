<template>
  <div class="card">
    <div class="pb-0 card-header mb-0">
      <h6>{{ '시간별 출근 기록' }}</h6> 
    </div>
    <div class="p-3 card-body">
	<canvas id="chart-line" class="chart-canvas" height="300"></canvas>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const commute = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("http://localhost:3000/attendancetime");
    commute.value = await response.json();
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});
</script>

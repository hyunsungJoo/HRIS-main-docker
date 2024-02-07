<template>
  <div class="row">
    <div class="col-lg-7 mb-lg-0 mb-4">
      <div class="card">
        <div class="p-3 pb-0 card-header">
          <div class="d-flex justify-content-between">
            <h6 class="mb-2">직급별 평균 급여</h6>
          </div>
        </div>
        <div class="table-responsive">
          <table class="table align-items-center">
            <thead>
              <tr>
                <th>직급</th>
                <th>평균 급여</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in positionAverages" :key="index">
                <td>{{ item.position }}</td>
                <td>{{ item.averageSalary.toFixed(2) }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="col-lg-5">
      <div class="card">
        <div class="pb-0 card-header mb-0">
          <h6>직급별 평균 급여 그래프</h6>
        </div>
        <div class="p-3 card-body">
          <canvas id="position-salary-chart" class="chart-canvas" height="300"></canvas>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Chart from "chart.js/auto";

const positionAverages = ref([]);

onMounted(async () => {
    try {
        const response = await fetch("https://hris-json-server.fly.dev/employees");
        const employees = await response.json();

        const averages = calculatePositionAverages(employees);
        positionAverages.value = averages;

        // 차트 생성
        createChart();
    } catch (error) {
        console.error("Error fetching data:", error);
    }
});

function calculatePositionAverages(employees) {
    const positionSalaryTotals = {};
    const positionEmployeeCounts = {};

    for (const employee of employees) {
        const { position, salary } = employee;
        
        // 급여를 숫자로 변환
        const numericSalary = Number(salary);
        // 유효한 숫자인 경우에만 계산에 포함
        if (!isNaN(numericSalary)) {
            if (position in positionSalaryTotals) {
                positionSalaryTotals[position] += numericSalary;
                positionEmployeeCounts[position]++;
            } else {
                positionSalaryTotals[position] = numericSalary;
                positionEmployeeCounts[position] = 1;
            }
        }
    }

    const averages = [];
    for (const position in positionSalaryTotals) {
        // 직원 수로 나누어 평균을 계산 후 객체 형태로 저장
        averages.push({ position, averageSalary: positionSalaryTotals[position] / positionEmployeeCounts[position] });
    }
    
    // 평균 급여에 따라 내림차순 정렬
    averages.sort((a, b) => b.averageSalary - a.averageSalary);

    return averages;
}

function createChart() {
    const ctx = document.getElementById('position-salary-chart').getContext('2d');
    const labels = positionAverages.value.map(item => item.position);
    const data = positionAverages.value.map(item => item.averageSalary);

    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: labels,
            datasets: [{
                label: '평균 급여',
                backgroundColor: 'rgba(94, 114, 228, 0.5)',
                borderColor: 'rgba(94, 114, 228, 1)',
                borderWidth: 1,
                data: data
            }]
        },
        options: {
            scales: {
                y: {
                    beginAtZero: true
                }
            }
        }
    });
}
</script>
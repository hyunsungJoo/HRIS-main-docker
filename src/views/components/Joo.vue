<template>
  <div class="col-lg-7 mb-lg-0 mb-4">
    <div class="card">
      <div class="p-3 pb-0 card-header">
        <div class="d-flex justify-content-between">
          <h6 class="mb-2">부서별 평균 급여</h6>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table align-items-center">
          <thead>
            <tr>
              <th>부서</th>
              <th>평균 급여</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(average, department) in departmentAverages" :key="department">
              <td>{{ department }}</td>
              <td>{{ average.toFixed(2) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
  <div class="col-lg-5">
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const attendancetimes = ref([]);
const departmentAverages = ref({});

onMounted(async () => {
    try {
        const response = await fetch("https://hris-json-server.fly.dev/attendancetime");
        const data = await response.json();
        attendancetimes.value = data;

        // 부서별 평균 연봉 계산 후 departmentAverages 업데이트
        const averages = calculateDepartmentAverages(data);
        departmentAverages.value = averages;
    } catch (error) {
        console.error("Error fetching data:", error);
    }
});

// 부서별 평균 연봉 계산 함수
function calculateDepartmentAverages(attendancetimes) {
    const departmentSalaryTotals = {};
    const departmentEmployeeCounts = {};

    for (const item of attendancetimes) {
        const { department, salary } = item;
        if (department && salary) { // 데이터 검증 추가
            if (!departmentSalaryTotals[department]) {
                departmentSalaryTotals[department] = 0;
                departmentEmployeeCounts[department] = 0;
            }
            departmentSalaryTotals[department] += salary;
            departmentEmployeeCounts[department] += 1;
        }
    }

    const averages = {};
    for (const department in departmentSalaryTotals) {
        averages[department] = departmentSalaryTotals[department] / departmentEmployeeCounts[department];
    }
    return averages;
}
</script>


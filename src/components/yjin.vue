<template>
  <div class="card">
    <div class="p-3 pb-0 card-header">
      <div class="d-flex justify-content-between">
        <h6 class="mb-2">부서별 성과</h6>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table text-center">
        <thead>
          <tr>
            <th>부서 이름</th>
            <th>부서 성과</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(department, index) in top5Departments" :key="index">
            <td class="text-center">{{ department.departmentName }}</td>
            <td class="text-center">{{ department.departmentPerformance }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const departments = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("https://hris-json-server.fly.dev/departmentalSales");
    departments.value = await response.json();
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});

const top5Departments = computed(() => { //eslint-disable-line no-unused-vars
  return departments.value
    .slice()
    .sort((a, b) => b.departmentPerformance - a.departmentPerformance)
    .slice(0, 5);
});
</script>
<template>
  <div class="card">
    <div class="card-header pb-0">
      <h6>사원리스트</h6>
    </div>

    <div class="card-body px-0 pt-0 pb-2">
      <div class="pb-5 w-50" style="margin: 0 auto;">
        <div class="input-group">
          <span class="input-group-text text-body">
            <i class="fas fa-search" aria-hidden="true"></i>
          </span>
          <input type="text" class="form-control input" @keyup="searchEmployee($event.target.value)" placeholder="이름을 입력해주세요." />
        </div>
      </div>

      <div class="d-flex justify-content-end">
        <button class="btn btn-primary" @click="toggleSortOrder()">
          {{ sortOrder === 'asc' ? '오래된 순서' : '최신 순서' }}
        </button>
      </div>

      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0">
          <thead>
            <tr>
              <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">이름</th>
              <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7 ps-2">부서</th>
              <th class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Status</th>
              <th class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">입사일</th>
              <th class="text-secondary opacity-7"></th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="(employee, index) in filteredEmployees" :key="index">
              <td>
                <div class="d-flex px-2 py-1">
                  <div>
                    <img src="../../assets/img/team-2.jpg" class="avatar avatar-sm me-3" alt="user1"/>
                  </div>
                  <div class="d-flex flex-column justify-content-center">
                    <h6 class="mb-0 text-sm">{{ employee.name }}</h6>
                  </div>
                </div>
              </td>
              <td>
                <p class="text-xs font-weight-bold mb-0">{{ employee.department }}</p>
                <p class="text-xs text-secondary mb-0">{{ employee.position }}</p>
              </td>
              <td class="align-middle text-center text-sm">
                <span class="badge badge-sm bg-gradient-success">Online</span>
              </td>
              <td class="align-middle text-center">
                <span class="text-secondary text-xs font-weight-bold">{{ employee.hireDate }}</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue';

const employees = ref([]);
const filteredEmployees = ref([]);
const sortOrder = ref('asc'); // 초기 정렬 순서

onMounted(async () => {
  try {
    const response = await fetch("https://hris-json-server.fly.dev/employees");
    employees.value = await response.json();

    // 초기에 필터된 목록은 전체 목록으로 설정합니다.
    filteredEmployees.value = employees.value;

  } catch (error) {
    console.error("Error fetching data:", error);
  }
});

// eslint-disable-next-line no-unused-vars
function searchEmployee(keyword){
  filteredEmployees.value = employees.value.filter(employee =>
    employee.name.toLowerCase().includes(keyword.toLowerCase())
  );
}

// eslint-disable-next-line no-unused-vars
function toggleSortOrder() {
  sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc';
  sortEmployees();
}

// eslint-disable-next-line no-unused-vars
function sortEmployees() {
  if (sortOrder.value === 'asc') {
    filteredEmployees.value.sort((a, b) => new Date(a.hireDate) - new Date(b.hireDate));
  } else {
    filteredEmployees.value.sort((a, b) => new Date(b.hireDate) - new Date(a.hireDate));
  }
}
</script>

<style scoped>
.card { min-height: 500px; }
</style>

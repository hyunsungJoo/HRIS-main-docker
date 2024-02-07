<template>
  <div class="card">
    <div class="card-header pb-0">
      <h6>사원리스트</h6>
    </div>



    <div class="card-body px-0 pt-0 pb-2">
      <div class=" pb-5 w-50" style="margin: 0 auto;">
        <div class="input-group">
          <span class="input-group-text text-body">
            <i class="fas fa-search" aria-hidden="true"></i>
          </span>
          <input type="text" class="form-control" />
        </div>
      </div>

      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0">
          <thead>
            <tr>
              <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">이름</th>
              <th
                class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7 ps-2"
              >부서</th>
              <th
                class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7"
              >Status</th>
              <th
                class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7"
              >입사일</th>
              <th class="text-secondary opacity-7"></th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="(employee, index) in employees" :key="index">
              <td>
                <div class="d-flex px-2 py-1">
                  <div>
                    <img
                      src="../../assets/img/team-2.jpg"
                      class="avatar avatar-sm me-3"
                      alt="user1"
                    />
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
                <span class="text-secondary text-xs font-weight-bold">{{employee.hireDate}}</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "authors-table",
};
</script>

<script setup>
import { ref, onMounted } from 'vue';

const employees = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("https://hris-json-server.fly.dev/employees");
    employees.value = await response.json();
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});


</script>

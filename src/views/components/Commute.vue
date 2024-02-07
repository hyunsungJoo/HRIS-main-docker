<template>
  <div class="card">
    <div class="p-3 pb-0 card-header">
      <div class="d-flex justify-content-between">
        <h6 class="mb-2">금일 시간별 출근자 수</h6>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table text-center">
        <thead>
          <tr>
            <th>출근 시간</th>
            <th>해당 시간 출근자 수</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>8시</td>
            <td>{{ countBefore8AM }}</td>
          </tr>
          <tr>
            <td>8시30분</td>
            <td>{{ countBetween801And830 }}</td>
          </tr>
          <tr>
            <td>9시</td>
            <td>{{ countBetween831And9 }}</td>
          </tr>
          <tr>
            <td>9시30분</td>
            <td>{{ countBetween901And930 }}</td>
          </tr>
          <tr>
            <td>10시</td>
            <td>{{ countBetween931And10 }}</td>
          </tr>
          <tr>
            <td>10시30분</td>
            <td>{{ countBetween1001And1030 }}</td>
          </tr>
          <tr>
            <td>11시</td>
            <td>{{ countBetween1031And11 }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const commute = ref([]);
    const countBefore8AM = ref(0);
    const countBetween801And830 = ref(0);
    const countBetween831And9 = ref(0);
    const countBetween901And930 = ref(0);
    const countBetween931And10 = ref(0);
    const countBetween1001And1030 = ref(0);
    const countBetween1031And11 = ref(0);
    const today = ref(getTodayDate());

    onMounted(async () => {
      try {
        const response = await fetch("https://hris-json-server.fly.dev/attendancetime");
        commute.value = await response.json();

        countBefore8AM.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          return startTimeHour < 8 && entry.date === today.value;
        }).length;

        countBetween801And830.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          const startTimeMinute = parseInt(entry.startTime.split(':')[1]);
          return (
            startTimeHour === 8 && startTimeMinute >= 0 && startTimeMinute <= 30 &&
            entry.date === today.value
          );
        }).length;

        countBetween831And9.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          const startTimeMinute = parseInt(entry.startTime.split(':')[1]);
          return (
            startTimeHour === 8 && startTimeMinute > 30 && startTimeHour < 9 &&
            entry.date === today.value
          );
        }).length;

        countBetween901And930.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          const startTimeMinute = parseInt(entry.startTime.split(':')[1]);
          return (
            startTimeHour === 9 && startTimeMinute >= 1 && startTimeMinute <= 30 &&
            entry.date === today.value
          );
        }).length;

        countBetween931And10.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          const startTimeMinute = parseInt(entry.startTime.split(':')[1]);
          return (
            startTimeHour === 9 && startTimeMinute > 30 && startTimeHour < 10 &&
            entry.date === today.value
          );
        }).length;

        countBetween1001And1030.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          const startTimeMinute = parseInt(entry.startTime.split(':')[1]);
          return (
            startTimeHour === 10 && startTimeMinute >= 1 && startTimeMinute <= 30 &&
            entry.date === today.value
          );
        }).length;

        countBetween1031And11.value = commute.value.filter(entry => {
          const startTimeHour = parseInt(entry.startTime.split(':')[0]);
          const startTimeMinute = parseInt(entry.startTime.split(':')[1]);
          return (
            startTimeHour === 10 && startTimeMinute > 30 && startTimeHour < 11 &&
            entry.date === today.value
          );
        }).length;
      } catch (error) {
        console.error("Error fetching data:", error.message);
      }
    });

    function getTodayDate() {
      const today = new Date();
      const year = today.getFullYear();
      const month = String(today.getMonth() + 1).padStart(2, '0');
      const day = String(today.getDate()).padStart(2, '0');
      return `${year}-${month}-${day}`;
    }

    return {
      today,
      countBefore8AM,
      countBetween801And830,
      countBetween831And9,
      countBetween901And930,
      countBetween931And10,
      countBetween1001And1030,
      countBetween1031And11
    };
  },
};
</script>

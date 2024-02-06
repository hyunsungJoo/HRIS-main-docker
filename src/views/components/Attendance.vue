<template>
	<div class="col-lg-3 col-md-6 col-12">
	  <div class="mb-4 card">
		<div class="p-3 card-body">
		  <div class="d-flex flex-row-reverse justify-content-between">
			<div></div>
			<div class="contentClass">
			  <div class="numbers">
				<p class="mb-0 text-sm text-uppercase font-weight-bold">금일날짜</p>
				<div v-for="(attendancetime, index) in attendancetimes" :key="index">
				  <!-- <h5 class="font-weight-bolder">{{ attendancetime.date }}</h5> -->
				</div>
			  </div>
			</div>
		  </div>
		</div>
	  </div>
	</div>

	<div class="col-lg-3 col-md-6 col-12">
	  <div class="mb-4 card">
		<div class="p-3 card-body">
		  <div class="d-flex flex-row-reverse justify-content-between">
			<div></div>
			<div class="contentClass">
			  <div class="numbers">
				<p class="mb-0 text-sm text-uppercase font-weight-bold">연차자</p>
				<div>
				  <h5 class="font-weight-bolder">{{  annualLeaveCountToday }}</h5>
				</div>
			  </div>
			</div>
		  </div>
		</div>
	  </div>
	</div>
  </template>

  <script setup>
  import { ref, onMounted } from 'vue';

  const attendancetimes = ref([]);
  const annualLeaveCountToday = ref(0);

  onMounted(async () => {
	try {
	  const response = await fetch("http://localhost:3000/attendancetime");
	  attendancetimes.value = await response.json();
	  console.log("Attendance times:", attendancetimes.value);

	  // 오늘의 연차자 수 계산
	  annualLeaveCountToday.value = countAnnualLeaveEmployeesToday(attendancetimes.value);
	  console.log("Annual Leave Employees Count Today:", annualLeaveCountToday.value);

	} catch (error) {
	  console.error("Error fetching data:", error);
	}
  });

  // 오늘의 연차자 수 계산 함수
  function countAnnualLeaveEmployeesToday(attendancetimes) {
	  const todayDate = getTodayDate();
	  let count = 0;
	  for (const record of attendancetimes) {
		  if (record.date === todayDate && record.annualLeave === true) {
			  count++;
		  }
	  }
	  return count;
  }

  // 오늘의 날짜 가져오기
  function getTodayDate() {
	  const today = new Date();
	  const year = today.getFullYear();
	  const month = String(today.getMonth() + 1).padStart(2, '0');
	  const day = String(today.getDate()).padStart(2, '0');
	  return `${year}-${month}-${day}`;
  }
  </script>

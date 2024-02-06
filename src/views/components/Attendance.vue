<template>
	<div class="col-lg-3 col-md-6 col-12">
		<div class="mb-4 card">
			<div class="p-3 card-body">
				<div class="d-flex flex-row-reverse justify-content-between">
					<div></div>
					<div class="contentClass">
						<div class="numbers">
							<p class="mb-0 text-sm text-uppercase font-weight-bold">Today</p>
							<p>{{ getTodayDate() }}</p>
							<!-- <div v-for="(attendancetime, index) in attendancetimes" :key="index">
								<h5 class="font-weight-bolder">{{ attendancetime.date }}</h5>
							</div> -->
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
							<p class="mb-0 text-sm text-uppercase font-weight-bold">오늘 연차 인원수</p>
							<div>
								<h5 class="font-weight-bolder">{{ annualLeaveCountToday }}</h5>
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
							<p class="mb-0 text-sm text-uppercase font-weight-bold">총 재택근무자</p>
							<div>
								<h5 class="font-weight-bolder">{{  remoteEmployeeCount  }}</h5>
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
	const remoteEmployeeCount = ref(0);

	onMounted(async () => {
		try {
			const response = await fetch("https://hris-json-server.fly.dev/attendancetime");
			attendancetimes.value = await response.json();

			// 오늘의 연차자 수 계산
			annualLeaveCountToday.value = countAnnualLeaveEmployeesToday(attendancetimes.value);

			// 재택근무자 수 계산
			remoteEmployeeCount.value = countRemoteEmployees(attendancetimes.value);
		} catch (error) {
			console.error("Error fetching data:", error);
		}
	});


	// 오늘날짜
	function getTodayDate() {
		const today = new Date();
		const year = today.getFullYear();
		const month = String(today.getMonth() + 1).padStart(2, '0');
		const day = String(today.getDate()).padStart(2, '0');
		return `${year}-${month}-${day}`;
	}

	// 오늘의 연차자 수 계산 함수
	function countAnnualLeaveEmployeesToday(attendancetimes) {
		const todayDate = getTodayDate();
		let count = 0;

		for (const item of attendancetimes) {
			if (item.date === todayDate && item.annualLeave === true) {
				count++;
			}
		}
		return count;
	}

	//재택근무자
	function countRemoteEmployees(attendancetimes){
		let count = 0;

		for (const item of attendancetimes) {
			if (item.remote === true) {
				count++;
			}
		}
		return count;
	}


</script>
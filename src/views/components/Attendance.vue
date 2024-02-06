<script setup>


</script>
<template>
<!--
금일 연차자 인원수가 보여진다.
평균 근무시간이 보여진다.
신규 입사자가 보여진다.
재택근무자 인원을 보여진다

-->
	<div class="col-lg-3 col-md-6 col-12">
		<div class="mb-4 card">
			<div class="p-3 card-body">
				<div class="d-flex flex-row-reverse justify-content-between">
					<!-- 아이콘  -->
					<div></div>
					<!-- <div>
						<div class="text-center icon icon-shape" :class="`${iconBackground} ${this.$store.state.isRTL ? 'border-radius-md' : 'border-radius-2xl'}`">
							<i class="text-lg opacity-10" :class="iconClass" aria-hidden="true"></i>
						</div>
					</div> -->
					<div class="contentClass">
						<div class="numbers">
							<p class="mb-0 text-sm text-uppercase font-weight-bold" >금일 연차자</p>
								<h5 class="font-weight-bolder" :class="valueColor">
									4
								</h5>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>



</template>
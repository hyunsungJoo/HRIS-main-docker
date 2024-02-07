<template>
  <div class="card card-carousel overflow-hidden h-100 p-0">
    <div id="carouselExampleCaptions" class="carousel slide h-100" data-bs-ride="carousel">
      <div class="carousel-inner border-radius-lg h-100">
        <div
          class="carousel-item h-100 active"
          :style="{backgroundImage: 'url(' + require('@/assets/img/carousel-1.jpg') + ')',
      backgroundSize: 'cover'}"
        >
          <div class="carousel-caption d-none d-md-block bottom-0 text-start start-0 ms-5">
            <div class="icon icon-shape icon-sm bg-white text-center border-radius-md mb-3">
              <i class="ni ni-camera-compact text-dark opacity-10"></i>
            </div>
            <h5 class="text-white mb-1">인사부</h5>
            <p>좋은 하루되세요!</p>
          </div>
        </div>
        <div
          class="carousel-item h-100"
          :style="{backgroundImage: 'url(' + require('@/assets/img/carousel-2.jpg') + ')',
      backgroundSize: 'cover'}"
        >
          <div class="carousel-caption d-none d-md-block bottom-0 text-start start-0 ms-5">
            <div class="icon icon-shape icon-sm bg-white text-center border-radius-md mb-3">
              <i class="ni ni-bulb-61 text-dark opacity-10"></i>
            </div>
            <h5 class="text-white mb-1">부서별</h5>
            <p>부서별 급여와 성과 리스트를 확인할 수 있습니다.<br/>개인정보보안에 유의해주세요!</p>
          </div>
        </div>
        <div
          class="carousel-item h-100"
          :style="{backgroundImage: 'url(' + require('@/assets/img/carousel-3.jpg') + ')',
      backgroundSize: 'cover'}"
        >
          <div class="carousel-caption d-none d-md-block bottom-0 text-start start-0 ms-5">
            <div class="icon icon-shape icon-sm bg-white text-center border-radius-md mb-3">
              <i class="ni ni-trophy text-dark opacity-10"></i>
            </div>
            <h5 class="text-white mb-1">사원리스트</h5>
            <p>전체 사원 리스트가 보여집니다.</p>
          </div>
        </div>
      </div>
      <button
        class="carousel-control-prev w-5 me-3"
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide="prev"
      >
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Previous</span>
      </button>
      <button
        class="carousel-control-next w-5 me-3"
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide="next"
      >
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Next</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "carousel",
};
</script>

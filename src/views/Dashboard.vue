<template>
  <div class="py-4 container-fluid">
    <div class="row">
      <!-- rahee -->
      <Attendance />
    </div>

    <div class="row py-4">
      <div class="col-lg-12">
        <div class="row">
          <div class="col-lg-7 mb-lg">
            <!-- line chart -->
            <div class="card z-index-2">
              <Commute />
            </div>
          </div>
          <div class="col-lg-5">
            <carousel />
          </div>
        </div>
        <div class="row mt-4">
          <div class="col-lg-7 mb-lg">
            <Joo />
          </div>
          <div class="col-lg-5">
            <Yjinlee />
          </div>
        </div>
        <Yjinlee2 />
      </div>
    </div>

    <div class="row py-4">
      <div class="col-12">
        <authors-table />
      </div>
    </div>
  </div>
</template>
<script>
import Carousel from "./components/Carousel.vue";
import Attendance from "./components/Attendance.vue";
import Joo from "./components/Joo.vue";
import Commute from "./components/Commute.vue";
import AuthorsTable from "./components/AuthorsTable.vue";

import US from "@/assets/img/icons/flags/US.png";
import DE from "@/assets/img/icons/flags/DE.png";
import GB from "@/assets/img/icons/flags/GB.png";
import BR from "@/assets/img/icons/flags/BR.png";

import Yjinlee from "@/components/yjin.vue"
import Yjinlee2 from "@/components/yjin2.vue"

export default {
  name: "dashboard-default",
  data() {
    return {

      sales: {
        us: {
          country: "United States",
          sales: 2500,
          value: "$230,900",
          bounce: "29.9%",
          flag: US,
        },
        germany: {
          country: "Germany",
          sales: "3.900",
          value: "$440,000",
          bounce: "40.22%",
          flag: DE,
        },
        britain: {
          country: "Great Britain",
          sales: "1.400",
          value: "$190,700",
          bounce: "23.44%",
          flag: GB,
        },
        brasil: {
          country: "Brasil",
          sales: "562",
          value: "$143,960",
          bounce: "32.14%",
          flag: BR,
        },
      },
    };
  },
  components: {
    Carousel,
    Yjinlee,
    Yjinlee2,
    Attendance,
    Joo,
    Commute,
    AuthorsTable
  },
};
</script>
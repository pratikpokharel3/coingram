<script setup>
import VueApexCharts from "vue3-apexcharts";

const props = defineProps({
  currencies: {
    type: Array,
    required: true
  }
});

const length = props.currencies.length - 1;

const series = [
  {
    name: `Yesterday's Rate`,
    data: [
      props.currencies[length - 1]["rates"][1]["sell"],
      props.currencies[length - 1]["rates"][3]["sell"],
      props.currencies[length - 1]["rates"][2]["sell"],
      props.currencies[length - 1]["rates"][5]["sell"],
      props.currencies[length - 1]["rates"][6]["sell"]
    ]
  },
  {
    name: `Today's Rate`,
    data: [
      props.currencies[length]["rates"][1]["sell"],
      props.currencies[length]["rates"][3]["sell"],
      props.currencies[length]["rates"][2]["sell"],
      props.currencies[length]["rates"][5]["sell"],
      props.currencies[length]["rates"][6]["sell"]
    ]
  }
];

const options = {
  markers: {
    size: 5
  },
  chart: {
    toolbar: {
      show: false
    },
    id: "graph",
    zoom: {
      enabled: false
    }
  },
  dataLabels: {
    enabled: false
  },
  xaxis: {
    categories: ["USD", "GBP", "EUR", "AUD", "CAD"]
  }
};
</script>

<template>
  <div class="row g-0">
    <div class="col-12 card shadow p-4">
      <div class="row g-0">
        <div class="col-12">
          <span class="h5">Popular Currencies</span>
        </div>

        <div class="col-12 fs-7 mt-2">
          <div class="row g-0 justify-content-between">
            <div class="col-12 col-lg-5">
              <span class="fw-bold">USD</span> - United States Dollar
            </div>

            <div class="col-12 col-lg-5">
              <span class="fw-bold">GBP</span> - Great Britain Pound
            </div>

            <div class="col-12 col-lg-5 mt-1">
              <span class="fw-bold">EUR</span> - Euro
            </div>

            <div class="col-12 col-lg-5 mt-1">
              <span class="fw-bold">AUD</span> - Australian Dollar
            </div>

            <div class="col-12 col-lg-5 mt-1">
              <span class="fw-bold">CAD</span> - Canadian Dollar
            </div>
          </div>
        </div>

        <div class="12">
          <VueApexCharts
            type="bar"
            :series="series"
            :options="options"
          ></VueApexCharts>
        </div>
      </div>
    </div>
  </div>
</template>

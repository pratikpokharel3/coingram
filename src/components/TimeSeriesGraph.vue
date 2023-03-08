<script setup>
import { ref } from "vue";
import ApexCharts from "vue3-apexcharts";

const props = defineProps({
  currencies: {
    type: Array,
    required: true
  }
});

let countryList = [];

const length = props.currencies.length - 1;

props.currencies[length]["rates"].forEach(item => {
  countryList.push({
    iso3: item.currency.iso3,
    name: item.currency.name
  });
});

const weekday = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

let buyList = [];
let sellList = [];
let graphXAxis = [];

props.currencies.forEach(val => {
  const buy = parseFloat(val["rates"][1]["buy"]);
  const sell = parseFloat(val["rates"][1]["sell"]);
  const date = new Date(val["date"]);

  buyList.push(buy);
  sellList.push(sell);
  graphXAxis.push(weekday[date.getDay()]);
});

const series = ref([
  {
    name: "Buy",
    data: [...buyList]
  },
  {
    name: "Sell",
    data: [...sellList]
  }
]);

const options = {
  colors: ["#FCCF31", "#17ead9"],
  markers: {
    size: 5
  },
  chart: {
    id: "graph",
    zoom: {
      enabled: false
    },
    toolbar: {
      show: false
    }
  },
  xaxis: {
    categories: [...graphXAxis]
  },
  tooltip: {
    y: {
      formatter: function (val) {
        return "Rs. " + val;
      }
    }
  }
};

function changeTimeSeriesGraph(event) {
  const idx = countryList.findIndex(item => item.iso3 === event.target.value);

  let newSeriesOne = [];
  let newSeriesTwo = [];

  props.currencies.forEach(curr => {
    const buy = curr["rates"][idx]["buy"];
    const sell = curr["rates"][idx]["sell"];

    newSeriesOne.push(buy);
    newSeriesTwo.push(sell);
  });

  series.value = [
    {
      name: "Buy",
      data: [...newSeriesOne]
    },
    {
      name: "Sell",
      data: [...newSeriesTwo]
    }
  ];
}
</script>

<template>
  <div class="row g-0">
    <div class="col-12 card shadow p-4">
      <div class="row g-0 justify-content-between">
        <div class="col-12 col-lg-6">
          <span class="h5">Time Series Graph</span>
          <div class="fs-7">Base Currency: NPR</div>
        </div>

        <div class="col-7 col-lg-5 mt-2 mt-lg-0">
          <select
            class="form-select"
            aria-label="Select Country"
            @input="changeTimeSeriesGraph"
          >
            <option
              :value="item.iso3"
              :selected="item.iso3 === 'USD'"
              v-for="item in countryList"
              :key="item.iso3"
            >
              {{ item.name }}
            </option>
          </select>
        </div>

        <div cols="12" class="fs-7 mt-3 mt-xl-4">
          This graphs shows the exchange rate for the past 7 days (oldest to
          newest).
        </div>

        <div cols="12" class="mt-2">
          <ApexCharts type="line" :series="series" :options="options" />
        </div>
      </div>
    </div>
  </div>
</template>

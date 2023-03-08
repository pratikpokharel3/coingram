<script setup>
import { ref } from "vue";

import Appbar from "./components/Appbar.vue";
import PopularCurrencies from "./components/PopularCurrencies.vue";
import TimeSeriesGraph from "./components/TimeSeriesGraph.vue";
import ConvertCurrency from "./components/ConvertCurrency.vue";
import ExchangeRateTable from "./components/ExchangeRateTable.vue";

// import { payload } from "./mockData";

const loading = ref(true);
const apiError = ref(false);

let currencyList = [];
let latestCurrency = [];

function getData() {
  loading.value = true;
  apiError.value = false;

  const startDate = new Date(new Date().getTime() - 7 * 86400 * 1000)
    .toISOString()
    .substring(0, 10);

  const endDate = new Date().toISOString().substring(0, 10);

  fetch(
    `https://www.nrb.org.np/api/forex/v1/rates?page=1&per_page=10&from=${startDate}&to=${endDate}`
  )
    .then(response => {
      if (response.ok) {
        return response.json();
      } else {
        apiError.value = true;
        loading.value = false;
      }
    })
    .then(({ data: { payload } }) => {
      latestCurrency = payload[payload.length - 1];
      currencyList = payload;
      loading.value = false;
    });

  // currencyList = payload;
  // latestCurrency = payload[payload.length - 1];

  // setTimeout(() => {
  //   loading.value = false;
  //   apiError.value = false;
  // }, 1000);
}

getData();
</script>

<template>
  <Appbar />

  <div class="loading" v-if="loading">
    <div class="spinner-border" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>

  <template v-else>
    <div class="data-not-loaded" v-if="apiError">
      <div class="text-center">
        <div>Oops! Looks like data cannot be loaded.</div>
        <div class="mt-2">
          <button type="button" class="btn btn-sm btn-primary" @click="getData">
            Try again
          </button>
        </div>
      </div>
    </div>

    <div class="container py-5" style="flex-grow: 1" v-else>
      <div class="row g-0 justify-content-center justify-content-md-between">
        <div class="col-12 col-md-6 pe-md-4">
          <PopularCurrencies :currencies="currencyList" />
        </div>

        <div class="col-12 col-md-6 ps-md-4 mt-5 mt-md-0">
          <TimeSeriesGraph :currencies="currencyList" />
        </div>

        <div class="col-12 col-md-6 mt-5 pe-md-4">
          <ConvertCurrency :latest-currency-list="latestCurrency.rates" />
        </div>

        <div class="col-12 card shadow p-4 mt-5">
          <ExchangeRateTable :latest-currency="latestCurrency" />
        </div>
      </div>
    </div>
  </template>
</template>

<style>
.loading {
  flex-grow: 1;
  display: grid;
  place-items: center;
}

.data-not-loaded {
  flex-grow: 1;
  display: grid;
  place-items: center;
}
</style>

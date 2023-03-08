<script setup>
import { ref } from "vue";

const props = defineProps({
  latestCurrencyList: {
    type: Array,
    required: true
  }
});

const currencyOne = ref(1);
const currencyTwo = ref(parseFloat(props.latestCurrencyList[1]["sell"]));
const selectedCurrencyOne = ref("USD");
const selectedCurrencyTwo = ref("NPR");

let rate1 = currencyTwo.value;
let unit1 = 1;
let rate2 = 1;
let unit2 = 1;

const currencyList = [];

props.latestCurrencyList.forEach(item => {
  const rate = parseFloat(item.sell);

  currencyList.push({
    rate,
    unit: item.currency.unit,
    iso3: item.currency.iso3,
    name: item.currency.name
  });
});

currencyList.unshift({
  rate: 1,
  unit: 1,
  iso3: "NPR",
  name: "Nepalese Rupee"
});

function changeCurrency(type) {
  if (type === 1) {
    const item = currencyList.find(
      item => item.iso3 === selectedCurrencyOne.value
    );

    rate1 = item.rate;
    unit1 = item.unit;

    convertCurrency(2);
  } else {
    const item = currencyList.find(
      item => item.iso3 === selectedCurrencyTwo.value
    );

    rate2 = item.rate;
    unit2 = item.unit;

    convertCurrency(1);
  }
}

function convertCurrency(type) {
  if (type === 1) {
    if (currencyOne.value === "" || currencyOne.value < 1) {
      return;
    }
    currencyTwo.value = (
      (rate1 * currencyOne.value * unit2) /
      (unit1 * rate2)
    ).toFixed(2);
  } else {
    if (currencyTwo.value === "" || currencyTwo.value < 1) {
      return;
    }
    currencyOne.value = (
      (unit1 * currencyTwo.value * rate2) /
      (rate1 * unit2)
    ).toFixed(2);
  }
}

function resetCurrency() {
  selectedCurrencyOne.value = "USD";
  selectedCurrencyTwo.value = "NPR";

  currencyOne.value = 1;
  currencyTwo.value = parseFloat(props.latestCurrencyList[1]["sell"]);

  rate1 = currencyTwo.value;
  unit1 = 1;
  rate2 = 1;
  unit2 = 1;
}
</script>

<template>
  <div class="row g-0">
    <div class="col-12 card shadow">
      <div class="row g-0 justify-content-between p-4">
        <div class="col-10">
          <span class="h5">Convert Currency</span>
        </div>

        <div class="col-2 text-end">
          <svg
            class="hover"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
            style="width: 24px; height: 24px"
            @click="resetCurrency"
          >
            <title>Reset</title>
            <path
              d="M17.65,6.35C16.2,4.9 14.21,4 12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20C15.73,20 18.84,17.45 19.73,14H17.65C16.83,16.33 14.61,18 12,18A6,6 0 0,1 6,12A6,6 0 0,1 12,6C13.66,6 15.14,6.69 16.22,7.78L13,11H20V4L17.65,6.35Z"
            />
          </svg>
        </div>

        <div class="col-6 mt-2">
          <select
            class="form-select"
            aria-label="Select Currency"
            v-model="selectedCurrencyOne"
            @change="changeCurrency(1)"
          >
            <option
              :value="item.iso3"
              :selected="item.iso3 === 'USD'"
              v-for="item in currencyList"
              :key="item.iso3"
            >
              {{ item.name }}
            </option>
          </select>
        </div>

        <div class="col-5 mt-2">
          <input
            type="number"
            class="form-control"
            aria-label="Enter Currency"
            v-model="currencyOne"
            @input="convertCurrency(1)"
          />
        </div>

        <div class="col-6 mt-2">
          <select
            class="form-select"
            aria-label="Select Currency"
            v-model="selectedCurrencyTwo"
            @change="changeCurrency(2)"
          >
            <option
              :value="item.iso3"
              :selected="item.iso3 === 'NPR'"
              v-for="item in currencyList"
              :key="item.iso3"
            >
              {{ item.name }}
            </option>
          </select>
        </div>

        <div class="col-5 mt-2">
          <input
            type="number"
            class="form-control"
            aria-label="Enter Currency"
            v-model="currencyTwo"
            @input="convertCurrency(2)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

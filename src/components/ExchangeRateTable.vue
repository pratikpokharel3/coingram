<script setup>
import { ref } from "vue";

const props = defineProps({
  latestCurrency: {
    type: Object,
    required: true
  }
});

const search = ref("");
const currencyList = ref(props.latestCurrency.rates);

function searchCurrency() {
  if (search.value === "") {
    currencyList.value = props.latestCurrency.rates;
    return;
  }

  currencyList.value = props.latestCurrency.rates.filter(item =>
    item.currency.name.toLowerCase().includes(search.value.toLowerCase())
  );
}
</script>

<template>
  <div class="row g-0 justify-content-between align-items-center">
    <div class="col-12 col-sm-5">
      <span class="h5">Exchange Rates</span>
      <div class="fs-7 mt-1">Updated At: {{ props.latestCurrency.date }}</div>
    </div>

    <div class="col-12 col-sm-6 col-lg-4 col-xl-3 mt-2">
      <div class="input-group">
        <input
          type="text"
          class="form-control"
          placeholder="Search Currency"
          aria-label="Search Currency"
          v-model="search"
          @input="searchCurrency"
        />

        <span class="input-group-text">
          <svg
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
            style="width: 24px; height: 24px"
          >
            <title>magnify</title>
            <path
              d="M9.5,3A6.5,6.5 0 0,1 16,9.5C16,11.11 15.41,12.59 14.44,13.73L14.71,14H15.5L20.5,19L19,20.5L14,15.5V14.71L13.73,14.44C12.59,15.41 11.11,16 9.5,16A6.5,6.5 0 0,1 3,9.5A6.5,6.5 0 0,1 9.5,3M9.5,5C7,5 5,7 5,9.5C5,12 7,14 9.5,14C12,14 14,12 14,9.5C14,7 12,5 9.5,5Z"
            />
          </svg>
        </span>
      </div>
    </div>

    <div cols="12" class="mt-3">
      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">Currency Name</th>
            <th scope="col">Country Code</th>
            <th scope="col">Unit</th>
            <th scope="col">Buy (Rs.)</th>
            <th scope="col">Sell (Rs.)</th>
          </tr>
        </thead>
        <tbody>
          <template v-if="currencyList.length !== 0">
            <tr v-for="item in currencyList" :key="item.currency.iso3">
              <td class="py-3">{{ item.currency.name }}</td>
              <td class="py-3">{{ item.currency.iso3 }}</td>
              <td class="py-3">{{ item.currency.unit }}</td>
              <td class="py-3">{{ item.buy }}</td>
              <td class="py-3">{{ item.sell }}</td>
            </tr>
          </template>

          <tr v-else>
            <td colspan="5" class="text-center py-3">No search found.</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

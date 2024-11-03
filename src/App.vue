<script setup>
import { ref } from "vue";

// import HelloWorld from './components/HelloWorld.vue'
const income = ref(0);
const result = ref(0);
const netIncome = ref(0);

// Function to format the number with commas
const formatCurrency = (value) => {
  return new Intl.NumberFormat("id-ID", {
    style: "currency",
    currency: "IDR",
    minimumFractionDigits: 0,
  }).format(value);
};

const calculateTax = () => {
  let tax = 0;
  const annualIncome = income.value;

  if (annualIncome <= 60000000) {
    tax = annualIncome * 0.05;
  } else if (annualIncome <= 250000000) {
    tax = 60000000 * 0.05 + (annualIncome - 60000000) * 0.15;
  } else if (annualIncome <= 500000000) {
    tax =
      60000000 * 0.05 +
      (250000000 - 60000000) * 0.15 +
      (annualIncome - 250000000) * 0.25;
  } else if (annualIncome <= 5000000 * 1000000) {
    // 5 M = 5,000,000,000
    tax =
      60000000 * 0.05 +
      (250000000 - 60000000) * 0.15 +
      (500000000 - 250000000) * 0.25 +
      (annualIncome - 500000000) * 0.3;
  } else {
    tax =
      60000000 * 0.05 +
      (250000000 - 60000000) * 0.15 +
      (500000000 - 250000000) * 0.25 +
      (5000000 * 1000000 - 500000000) * 0.3 +
      (annualIncome - 5000000 * 1000000) * 0.35;
  }

  result.value = tax;
  netIncome.value = income.value - tax;
};
</script>

<template>
  <div>
    <div class="mb-3">
      <label class="form-label">Masukkan Jumlah Income Pertahun</label>
      <input type="number" class="form-control" v-model.number="income" />
    </div>
    <button class="btn btn-primary" @click="calculateTax">Hitung</button>
  </div>
  <div class="mt-3 mb-1">
    <label class="form-label">Income: {{ formatCurrency(income) }}</label>
  </div>
  <div class="mb-1">
    <label class="form-label"
      >Pajak yang harus dibayar: {{ formatCurrency(result) }}</label
    >
  </div>
  <div>
    <label class="form-label"
      >Nett Income: {{ formatCurrency(netIncome) }}</label
    >
  </div>
</template>

<style scoped></style>

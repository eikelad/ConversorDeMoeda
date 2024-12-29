<template>
    <div class="bg-white shadow-lg rounded-lg p-6 w-96" style="box-shadow: 0 8px 16px rgba(0, 0, 0, 2);">
        <img class="mb-1 mx-auto w-40 h-40" src="../img/green-dolar.png" alt="Green Dollar Icon">

  
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-medium mb-2" for="amount">Quantia</label>
        <input
          v-model="amount"
          type="number"
          id="amount"
          class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
          placeholder="Insira a quantia"
        />
      </div>
  
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-medium mb-2">De</label>
        <select
          v-model="fromCurrency"
          class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
        >
          <option v-for="(rate, currency) in rates" :key="currency" :value="currency">
            {{ currency }}
          </option>
        </select>
      </div>
  
      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-medium mb-2">Para</label>
        <select
          v-model="toCurrency"
          class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
        >
          <option v-for="(rate, currency) in rates" :key="currency" :value="currency">
            {{ currency }}
          </option>
        </select>
      </div>
  
      <button
        @click="convertCurrency"
        class="w-full bg-green-500 text-white py-2 rounded-lg font-medium hover:bg-green-600"
      >
        Converter
      </button>
  
      <p
        v-if="convertedAmount !== null"
        class="mt-4 text-lg text-center font-semibold text-green-600"
      >
        {{ amount }} {{ fromCurrency }} = {{ convertedAmount }} {{ toCurrency }}
      </p>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, onMounted } from "vue";
  
  export default defineComponent({
    name: "CurrencyConverter",
    setup() {
      const amount = ref<number>(0);
      const fromCurrency = ref<string>("USD");
      const toCurrency = ref<string>("EUR");
      const rates = ref<Record<string, number>>({});
      const convertedAmount = ref<number | null>(null);
  
      const fetchRates = async () => {
        const response = await fetch("https://api.exchangerate-api.com/v4/latest/USD");
        const data = await response.json();
        rates.value = data.rates;
      };
  
      const convertCurrency = () => {
        const rate = rates.value[toCurrency.value] / rates.value[fromCurrency.value];
        convertedAmount.value = parseFloat((amount.value * rate).toFixed(2));
      };
  
      onMounted(fetchRates);
  
      return {
        amount,
        fromCurrency,
        toCurrency,
        rates,
        convertedAmount,
        convertCurrency,
      };
    },
  });
  </script>
  
  <style scoped>
  /* Adicione estilos adicionais, se necessário */
  </style>
  
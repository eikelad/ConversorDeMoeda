<template>
  <div class="min-h-screen flex items-center justify-center">
    <currency-converter />
  </div>
</template>


<script lang="ts">
import { defineComponent } from "vue";
import CurrencyConverter from "./components/CurrencyConverter.vue";

export default defineComponent({
  components: {
    CurrencyConverter,
  },
});
</script>


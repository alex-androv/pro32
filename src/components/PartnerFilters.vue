<template>
  <div class="partner-filters">
    <div class="row g-3 mb-4">
      <div class="col-md-6 d-flex">
        <select
          class="form-select"
          v-model="localFilters.country"
          @change="emitUpdate"
        >
          <option value="">Страна</option>
          <option
            v-for="country in countries"
            :key="country"
            :value="country"
          >
            {{ country }}
          </option>
        </select>
      </div>

      <div class="col-md-6">
        <select
          class="form-select"
          v-model="localFilters.city"
          @change="emitUpdate"
        >
          <option value="">Город</option>
          <option
            v-for="city in filteredCities"
            :key="city"
            :value="city"
          >
            {{ city }}
          </option>
        </select>
      </div>
    </div>

    <div class="product-type-toggle mb-4">
      <div
        class="btn-group"
        role="group"
      >
        <input
          type="radio"
          class="btn-check"
          name="productType"
          id="homeType"
          value="Для дома"
          v-model="localFilters.productType"
          @change="emitUpdate"
        />
        <label
          class="btn btn-outline-primary"
          for="homeType"
          >Для дома</label
        >

        <input
          type="radio"
          class="btn-check"
          name="productType"
          id="businessType"
          value="Для бизнеса"
          v-model="localFilters.productType"
          @change="emitUpdate"
        />
        <label
          class="btn btn-outline-primary"
          for="businessType"
          >Для бизнеса</label
        >
      </div>
    </div>

    <div class="products-filter mb-4">
      <div class="btn-group">
        <button
          v-for="product in products"
          :key="product"
          class="btn btn-outline-secondary"
          :class="{ active: localFilters.products.includes(product) }"
          @click="toggleProduct(product)"
        >
          {{ product }}
        </button>
      </div>
    </div>

    <div class="partner-types-filter mb-4">
      <div class="d-flex flex-wrap gap-2">
        <button
          v-for="type in partnerTypes"
          :key="type"
          class="btn btn-outline-secondary"
          :class="{ active: localFilters.partnerTypes.includes(type) }"
          @click="togglePartnerType(type)"
        >
          {{ type }}
        </button>
      </div>
    </div>

    <div class="d-flex gap-3">
      <button
        class="btn btn-outline-secondary"
        @click="clearFilters"
      >
        Очистить
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import partnersData from "../data/partners.json";

const props = defineProps({
  filters: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["update:filters", "clear"]);

const localFilters = ref({ ...props.filters });

const countries = computed(() => {
  return [...new Set(partnersData.map((p) => p.country))];
});

const filteredCities = computed(() => {
  const cities = partnersData
    .filter(
      (p) =>
        !localFilters.value.country || p.country === localFilters.value.country
    )
    .map((p) => p.city);
  return [...new Set(cities)];
});

const products = ["Антивирус", "GetScreen", "Passwork"];
const partnerTypes = [
  "Партнеры Retail",
  "Партнеры Corporate",
  "Интернет-провайдеры",
  "Online партнеры",
  "Продажи партнерам",
  "Education партнеры",
  "MSP Партнеры",
];

const toggleProduct = (product) => {
  const index = localFilters.value.products.indexOf(product);
  if (index === -1) {
    localFilters.value.products.push(product);
  } else {
    localFilters.value.products.splice(index, 1);
  }
  emitUpdate();
};

const togglePartnerType = (type) => {
  const index = localFilters.value.partnerTypes.indexOf(type);
  if (index === -1) {
    localFilters.value.partnerTypes.push(type);
  } else {
    localFilters.value.partnerTypes.splice(index, 1);
  }
  emitUpdate();
};

const emitUpdate = () => {
  emit("update:filters", { ...localFilters.value });
};

const clearFilters = () => {
  emit("clear");
};

watch(
  () => props.filters,
  (newFilters) => {
    localFilters.value = { ...newFilters };
  },
  { deep: true }
);
</script>
<style scoped>
.partner-filters {
  max-width: 30%;
}
</style>

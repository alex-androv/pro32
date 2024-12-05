<template>
  <div class="container py-4">
    <h1 class="text-center mb-4">Где купить</h1>
    <p class="text-center text-muted mb-5">Контакты наших партнеров</p>
    <div class="d-flex justify-content-between">
      <PartnerFilters 
        :filters="filters"
        @update:filters="updateFilters"
        @clear="clearFilters"

      />

      <div class="g-4">
        <template v-for="partner in filteredPartners" :key="partner.id">
          <PartnerCard :partner="partner" />
        </template>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import PartnerFilters from './components/PartnerFilters.vue'
import PartnerCard from './components/PartnerCard.vue'
import partnersData from './data/partners.json'

const partners = ref(partnersData)

const filters = ref({
  country: '',
  city: '',
  productType: 'Для дома',
  products: [],
  partnerTypes: []
})

const filteredPartners = computed(() => {
  return partners.value.filter(partner => {
    const countryMatch = !filters.value.country || partner.country === filters.value.country
    const cityMatch = !filters.value.city || partner.city === filters.value.city
    const productTypeMatch = partner.productType === filters.value.productType
    const productsMatch = !filters.value.products.length || 
      filters.value.products.some(p => partner.products.includes(p))
    const partnerTypesMatch = !filters.value.partnerTypes.length || 
      filters.value.partnerTypes.some(pt => partner.partnerType.includes(pt))

    return countryMatch && cityMatch && productTypeMatch && productsMatch && partnerTypesMatch
  })
})

const updateFilters = (newFilters) => {
  filters.value = { ...filters.value, ...newFilters }
}

const clearFilters = () => {
  filters.value = {
    country: '',
    city: '',
    productType: 'Для дома',
    products: [],
    partnerTypes: []
  }
}
</script>
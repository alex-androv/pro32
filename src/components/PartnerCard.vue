<template>
  <div class="partner-card mb-3">
    <div class="card h-100">
      <div class="card-body">
        <div class="d-flex justify-content-between align-items-start mb-3">
          <img 
            :src="getLogoUrl(partner.logo)" 
            :alt="partner.name"
            class="partner-logo"
            height="40"
          >
          <div class="">
          <h5 class="card-title mb-3">{{ partner.name }}</h5>
          
          <div class="card-text d-flex">
              <p class="mb-2 me-4">
              <i class="bi bi-telephone me-2"></i>
              {{ partner.phone }}
              </p>
              <p class="mb-2 me-4">
              <i class="bi bi-envelope me-2"></i>
              <a :href="'mailto:' + partner.email">{{ partner.email }}</a>
              </p>
              <p class="mb-0">
              <i class="bi bi-globe me-2"></i>
              <a 
                  :href="'https://' + partner.website" 
                  target="_blank"
                  rel="noopener noreferrer"
              >
                  {{ partner.website }}
              </a>
              </p>
          </div>
        </div>
          <span 
            class="badge"
            :class="partner.status === 'Premier' ? 'bg-primary' : 'bg-secondary'"
          >
            {{ partner.status }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  partner: {
    type: Object,
    required: true
  }
})

const getLogoUrl = (logo) => {
return new URL(`../assets/logos/${logo.toLowerCase()}.png`, import.meta.url).href
}
</script>

<style scoped>
.partner-card {
  width: 800px;
}
.partner-logo {
  object-fit: contain;
  max-width: 120px;
}

.card {
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-5px);
}
</style>
<script setup>
import { ref } from 'vue'
import Countries from './components/Countries.vue'
import EditCountryModal from './components/EditCountryModal.vue'

const countries = ref([])
const isModalOpen = ref(false)
const selectedCountry = ref(null)

const fetchCountries = async () => {
  try {
    const res = await fetch('https://restcountries.com/v3.1/all')
    countries.value = await res.json()
  } catch (error) {
    console.error('Erro ao buscar países:', error)
  }
}
fetchCountries()

const openModal = (country) => {
  selectedCountry.value = country
  isModalOpen.value = true
}

const updateCountry = (updatedCountry) => {
  const index = countries.value.findIndex(c => c.cca3 === updatedCountry.cca3)
  if (index !== -1) {
    countries.value[index] = updatedCountry
  }
}
</script>

<template>
  <div class="m-auto">
    <Countries
      :countries="countries"
      @open-modal="openModal"
    />
    <EditCountryModal
      v-model:isOpen="isModalOpen"
      :country="selectedCountry"
      @update-country="updateCountry"
    />
  </div>
</template>

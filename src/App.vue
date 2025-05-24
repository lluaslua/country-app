<script setup>
import { ref } from 'vue'
import Countries from './components/Countries.vue'
import EditCountryModal from './components/EditCountryModal.vue'
import SearchArea from './components/SearchArea.vue'

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
const deleteCountry = (cca3) => {
  countries.value = countries.value.filter(c => c.cca3 !== cca3)
}

const openModal = (country) => {
  selectedCountry.value = country
  isModalOpen.value = true
}

const updateCountry = (updatedCountry) => {
  const index = countries.value.findIndex(c => c.cca3 === updatedCountry.cca3)
  if (index !== -1) {
    countries.value[index] = updatedCountry
  }
  isModalOpen.value = false 
}
</script>

<template>

  <div class="flex flex-col items-center px-4 py-6">
    <div class="flex flex-col justify-center">
      <SearchArea/>
      <Countries
    :countries="countries"
    @open-modal="openModal"
    @delete-country="deleteCountry"
  />
      <EditCountryModal
        v-model:isOpen="isModalOpen"
        :country="selectedCountry"
        @update-country="updateCountry"
      />
    </div>
  </div>
</template>

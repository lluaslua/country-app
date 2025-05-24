<script setup>
import { ref } from 'vue'
import Countries from './components/Countries.vue'
import EditCountryModal from './components/EditCountryModal.vue'
import SearchArea from './components/SearchArea.vue'
import NewCountryModal from './components/NewCountryModal.vue'
import CountryDetailsModal from './components/CountryDetailsModal.vue'

const countries = ref([])
const isModalOpen = ref(false)
const newCountry = ref(null)
const isAddModalOpen = ref(false) 
const selectedCountry = ref(null)
const isDetailModalOpen = ref(false)


const detalhesAberto = ref(false);
const paisSelecionado = ref(null);


function handleOpenDetail(country) {
  paisSelecionado.value = country;
  detalhesAberto.value = true;
}

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

const addNewCountry = (newCountry) => {
  
  const country = {
    cca3: Date.now().toString(), 
    name: {
      official: newCountry.officialName,
      common: newCountry.commonName
    },
    area: newCountry.area,
    population: newCountry.population,
    region: newCountry.region,
    capital: [newCountry.capital],
    languages: { [newCountry.language]: newCountry.language },
    currencies: { [newCountry.currencies]: { name: newCountry.currencies } },
    flags: { png: newCountry.flags.png }
  }
  countries.value.unshift(country)
  isAddModalOpen.value = false
}


const openDetailModal = (country) => {
  selectedCountry.value = country
  isDetailModalOpen.value = true
}

const showDetailsModal = ref(false)

function openDetailsModal(country) {
  selectedCountry.value = country
  showDetailsModal.value = true
}

</script>

<template>

  <div class="flex flex-col items-center px-4 py-6">
    <div class="flex flex-col justify-center">
      <SearchArea @open-new-modal="isAddModalOpen = true"/>
      <Countries
    :countries="countries"
    @open-modal="openModal"
    @delete-country="deleteCountry"
    @open-details="openDetailsModal"
  />
      <EditCountryModal
        v-model:isOpen="isModalOpen"
        :country="selectedCountry"
        @update-country="updateCountry"
      />
      <NewCountryModal
        v-model:isOpen="isAddModalOpen"
        @create-country="addNewCountry"
      />
       <CountryDetailsModal
  v-model:isOpen="showDetailsModal"
  :country="selectedCountry"
/>
    </div>
  </div>
</template>

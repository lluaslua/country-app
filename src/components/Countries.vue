<script setup>
import { defineProps, defineEmits, computed } from 'vue'
import pencil from '../assets/pencil.svg'
import trash from '../assets/trash.svg'

const props = defineProps({
  countries: Array,
  search: String,
  filters: Object
})

const filteredCountries = computed(() => {
  return props.countries.filter(country => {
    const matchesSearch = !props.search || country.name.common.toLowerCase().includes(props.search.toLowerCase())

    const matchesContinent =
      !props.filters.continents.length || props.filters.continents.includes(country.region)

    const matchesPopulation =
      (props.filters.populationMin == null || country.population >= props.filters.populationMin) &&
      (props.filters.populationMax == null || country.population <= props.filters.populationMax)

    return matchesSearch && matchesContinent && matchesPopulation
  })
})

const emit = defineEmits(['open-modal', 'delete-country', 'open-details'])

function excludeCountry(cca3) {
  emit('delete-country', cca3)
} 

function handleRowClick(country) {
  emit('open-details', country)
}

</script>

<template>
  <div>
    <table class="text-left border border-gray-200 text-white bg-[#101C3D]">
        <thead>
            <tr>
                <th class="px-4 py-3 font-medium text-white text-left">Name</th>
                <th class="px-4 py-3 font-medium text-whute text-left">Region</th>
                <th class="px-4 py-3 font-medium text-white text-left">Capital</th>
                <th class="px-4 py-3 font-medium text-white text-left">population</th>
                <th class="px-4 py-3 font-medium text-white text-left">Actions</th>

            </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
            <tr 
          v-for="country in filteredCountries" 
          :key="country.cca3"
         @click="handleRowClick(country)"
          class="cursor-pointer hover:bg-blue-900/20 transition-colors"
        >
                <td class="flex flex-row gap-2 px-4 py-2  whitespace-nowrap "> <img :src="country.flags.png" :alt="`Bandeira de ${country.name.common}`" width="50" />
  {{ country.name.common }} </td>
                <td class="px-4 py-2  whitespace-nowrap">{{ country.region }}</td>
                <td class="px-4 py-2  whitespace-nowrap">{{ country.capital?.[0] }}</td>
                <td class="px-4 py-2  whitespace-nowrap">{{ country.population.toLocaleString() }}</td>
                <td class="px-4 py-2  whitespace-nowrap flex gap-1">
    <button @click.stop="emit('open-modal', country)" class="cursor-pointer bg-blue-900 hover:bg-blue-700 p-2 rounded-full w-8 h-8">
      <img :src="pencil" alt="Edit" class="w-4 h-4 text-blue-300 w-4 h-4" />
    </button><button @click.stop="excludeCountry(country.cca3)" class="cursor-pointer bg-blue-900 hover:bg-blue-700 p-2 rounded-full w-8 h-8">
      <img :src="trash" alt="Delete" class="w-4 h-4 text-blue-300 w-4 h-4" />
    </button></td>
                
            </tr>
        </tbody>
    </table>
  </div>
</template>
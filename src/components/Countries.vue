<script setup>
import { ref, onMounted } from 'vue'
import pencil from '../assets/pencil.svg'
import trash from '../assets/trash.svg'

const countries = ref([])
const population = ref([])

function excludeCountry(cca3) {
  const index = countries.value.findIndex(p => p.cca3 === cca3);
  if (index !== -1) {
    countries.value.splice(index, 1);
  }
}

onMounted(async () => {
  try {
    const res = await fetch('https://restcountries.com/v3.1/all')
    countries.value = await res.json()
  } catch (error) {
    console.error('Erro ao buscar países:', error)
  }
})
</script>

<template>
  <div class="w-full m-auto ">
    <table class="table-auto text-left border border-gray-200 text-white bg-[#101C3D] m-auto">
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
            <tr v-for="country in countries" :key="country.cca3">
                <td class="flex flex-row gap-2 px-4 py-2  whitespace-nowrap "> <img :src="country.flags.png" :alt="`Bandeira de ${country.name.common}`" width="50" />
  {{ country.name.common }} </td>
                <td class="px-4 py-2  whitespace-nowrap">{{ country.region }}</td>
                <td class="px-4 py-2  whitespace-nowrap">{{ country.capital?.[0] }}</td>
                <td class="px-4 py-2  whitespace-nowrap">{{ country.population.toLocaleString() }}</td>
                <td class="px-4 py-2  whitespace-nowrap flex gap-1"><button class="bg-blue-900 hover:bg-blue-700 p-2 rounded-full">
      <img :src="pencil" alt="Edit" class="w-4 h-4 text-blue-300" />
    </button><button @click="excludeCountry(country.cca3)" class="bg-blue-900 hover:bg-blue-700 p-2 rounded-full">
      <img :src="trash" alt="Delete" class="w-4 h-4 text-blue-300" />
    </button></td>
                
            </tr>
        </tbody>
    </table>
  </div>
</template>

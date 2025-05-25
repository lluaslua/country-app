<script setup>
import { ref, watch } from 'vue'


const emit = defineEmits(['update:isOpen'])


const props = defineProps({
  country: {
    type: Object,
    required: false,
    default: null
  },
  isOpen: Boolean,
})

const form = ref({
  officialName: '',
  commonName: '',
  area: '',
  population: '',
  region: '',
  capital: '',
  language: '',
  currencies: '',
  flags: { png: '' }
})



  
function closeModal() {
  emit('update:isOpen', false)
}

watch(() => props.isOpen, (val) => {
  if (val && props.country) {
    document.body.classList.add('overflow-hidden')
    form.value = {
      officialName: props.country.name.official || '',
      commonName: props.country.name.common || '',
      area: props.country.area?.toLocaleString() || '0', 
      population: props.country.population?.toLocaleString() || '0',
      region: props.country.region || '',
      capital: props.country.capital?.[0] || '-',
      language: Object.values(props.country.languages || {}).join(', ') || '-', 
      currencies: Object.values(props.country.currencies || {}).map(c => c.name).join(', ') || '-', 
      flags: { png: props.country.flags?.png || '' }
    }
  } else {
    document.body.classList.remove('overflow-hidden')
  }
})



</script>

<template>
  <div>
    <teleport to="#modal">
        <div v-if="isOpen" class="fixed inset-0 z-40 bg-gray-900 bg-opacity-70 backdrop-blur-sm"></div>
        <div v-if="isOpen" id="details-modal" tabindex="-1" aria-hidden="true" class="overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 flex justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full">
          <div class="relative p-4 w-full max-w-2xl max-h-full">

              <div class="relative bg-white rounded-lg shadow-sm dark:bg-gray-700">
                  
                  <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600 border-gray-200">
                      <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                          Country Details
                      </h3>
                      <button @click="closeModal" type="button" class="cursor-pointer text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white" data-modal-hide="default-modal">
                          <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
                          </svg>
                          <span class="sr-only">Close modal</span>
                      </button>
                  </div>
                 
                  <div class="p-4 md:p-5 space-y-4">
                      <div class="flex items-center gap-4">
                        <img
                          :src="form.flags.png"
                          alt="Country Flag"
                          class="w-40 h-24 object-contain border"
                        />

                     <form @submit.prevent="saveEdit" class="space-y-4">
                        <div class="flex gap-2">
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Official Name
                            </label>
                           <input type="text" v-model="form.officialName" id="official-name" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled />
                          </div>
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Common Name
                           
                            </label>
                           <input type="text" v-model="form.commonName" id="common-name" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled />
                          </div>
                        </div>



                        <div class="flex gap-2">
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white" >Area(km²)
                              
                            </label>
                           <input type="number" v-model="form.area" id="Area" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled />
                          </div>
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Population
                              
                            </label>
                           <input type="number" v-model="form.population" id="Population" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled />
                          </div>
                        </div>



                        <div class="flex gap-2">
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white" >Region
                              
                            </label>
                           <select v-model="form.region" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled >
                            <option disabled>Select a Region</option>
                            <option>Africa</option>
                            <option>Americas</option>
                            <option>Antarctica</option>
                            <option>Asia</option>
                            <option>Europe</option>
                            <option>Oceania</option>
                          </select>
                          </div>
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white" >Capital
                              
                            </label>
                           <input type="text" v-model="form.capital" id="Capital" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" disabled/>
                          </div>
                        </div>
                        <div>
                          <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"  >Language
                            
                          </label>
                         <input type="text" v-model="form.language" id="Language" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled/>
                        </div>
                        <div>
                          <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Currencies
                            
                          </label>
                         <input type="text" v-model="form.currencies" id="Currencies" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"  disabled/>
                        </div>
                     </form>
                  </div>



                  <div class="flex items-center space-x-2 justify-end p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600">
                    <button @click="closeModal" data-modal-hide="default-modal" type="button" class="cursor-pointer text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Close</button>
                  </div>
              </div>
          </div>
        </div>
      </div>
    </teleport>
  </div>
</template>
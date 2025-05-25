<script setup>
import { ref, watch } from 'vue'


const emit = defineEmits(['update:isOpen', 'create-country'])

const props = defineProps({
  isOpen: Boolean
})

const errors = ref({
  officialName: false,
  commonName: false,
  area: false,
  population: false,
  region: false,
  capital: false,
  language: false,
  currencies: false,
  flag: false
})

const validateForm = () => {
  let isValid = true
  const newErrors = {
    officialName: !form.value.officialName.trim(),
    commonName: !form.value.commonName.trim(),
    area: !form.value.area || isNaN(form.value.area) || form.value.area <= 0,
    population: !form.value.population || isNaN(form.value.population) || form.value.population <= 0,
    region: !form.value.region || form.value.region === 'Select a Region',
    capital: !form.value.capital.trim(),
    language: !form.value.language.trim(),
    currencies: !form.value.currencies.trim(),
    flag: !form.value.flags.png
  }

  errors.value = newErrors
  isValid = !Object.values(newErrors).some(error => error)
  return isValid
}

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
  if (val) {
    document.body.classList.add('overflow-hidden')
    form.value = {
      officialName: '',
      commonName: '',
      area: '',
      population: '',
      region: '',
      capital: '',
      language: '',
      currencies: '',
      flags: { png: '' }
    }
  } else {
    document.body.classList.remove('overflow-hidden')
  }
})

const createCountry = () => {
    if (!validateForm()) return

    emit('create-country', form.value)
    closeModal()
}

function handleFlagUpload(event) {
  const file = event.target.files[0]
  if (file) {
    form.value.flags = {
      png: URL.createObjectURL(file)
    }
  }
}


</script>

<template>
    <teleport to="#modal">
        <div v-if="isOpen" class="fixed inset-0 z-40 bg-gray-900 bg-opacity-70 backdrop-blur-sm"></div>
      <div
    v-if="isOpen"
    id="default-modal"
    tabindex="-1"
    aria-hidden="true"
    class="overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 flex justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full"
  >
          <div class="relative p-4 w-full max-w-2xl max-h-full">

              <div class="relative bg-white rounded-lg shadow-sm dark:bg-gray-700">
                  
                  <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600 border-gray-200">
                      <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                          Create New Country
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
    
    <label class="cursor-pointer">
      <div
        class="w-30 h-20 rounded flex items-center justify-center overflow-hidden"
        :class="form.flags?.png ? 'border' : 'border-2 border-dashed border-gray-400 bg-gray-100'"
      >
        <img
          v-if="form.flags?.png"
          :src="form.flags.png"
          alt="Country Flag"
          class="w-full h-full object-cover"
        />
      </div>
      
      <input
        type="file"
        accept="image/*"
        class="hidden"
        @change="handleFlagUpload"
      />
    </label>
    <span class="text-sm text-gray-500">Upload flag (PNG ou JPG)</span>
  </div>

                     <form @submit.prevent="createCountry" class="space-y-4">
                        <div class="flex gap-2">
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Official Name
                                <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                            </label>
                           <input type="text" v-model="form.officialName" id="official-name" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                          </div>
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Common Name
                                <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                            </label>
                           <input type="text" v-model="form.commonName" id="common-name" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                          </div>
                        </div>



                        <div class="flex gap-2">
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white" min>Area(km²)
                                <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                            </label>
                           <input type="number" v-model="form.area" id="Area" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                          </div>
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Population
                                <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                            </label>
                           <input type="number" v-model="form.population" id="Population" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                          </div>
                        </div>



                        <div class="flex gap-2">
                          <div class="flex-1">
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Region
                                <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                            </label>
                           <select v-model="form.region" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required>
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
                            <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Capital
                                <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                            </label>
                           <input type="text" v-model="form.capital" id="Capital" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                          </div>
                        </div>
                        <div>
                          <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Language
                            <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                          </label>
                         <input type="text" v-model="form.language" id="Language" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                        </div>
                        <div>
                          <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Currencies
                            <span v-if="errors.officialName" class="text-red-500 text-xs">*</span>
                          </label>
                         <input type="text" v-model="form.currencies" id="Currencies" class="block w-full p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required/>
                        </div>
                     </form>
                  </div>



                  <div class="flex items-center space-x-2 justify-end p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600">
                    <button @click="closeModal" data-modal-hide="default-modal" type="button" class="cursor-pointer py-2.5 px-5 ms-3 text-sm font-medium text-gray-900 focus:outline-none bg-white rounded-lg border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-gray-100 dark:focus:ring-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:text-white dark:hover:bg-gray-700">Cancel</button>
                    <button @click="createCountry" data-modal-hide="default-modal" type="button" class="cursor-pointer text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Save</button>
                  </div>
              </div>
          </div>
      </div>
    </teleport>
</template>
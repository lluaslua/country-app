<script setup>
import { ref } from 'vue'

const isFilterOpen = ref(false)



</script>

<template>
<div class="">
  <div class="relative">

    <button @click="isFilterOpen=true" id="dropdownCheckboxButton" data-dropdown-toggle="dropdownDefaultCheckbox" class="relative cursor-pointer text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800" type="button">Filters <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
    </svg>
  </button>
  

  <div v-if="isFilterOpen" id="dropdownDefaultCheckbox" class="absolute top-full right-0 z-10 w-fit bg-white divide-y divide-gray-100 rounded-lg shadow-sm dark:bg-gray-700 dark:divide-gray-600">
    <div class="flex items-center justify-between p-4 md:p-2 border-b rounded-t dark:border-gray-600 border-gray-200">
          <button @click="isFilterOpen=false" type="button" class="cursor-pointer text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white" data-modal-hide="default-modal">
              <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
              </svg>
              <span class="sr-only">Close modal</span>
          </button>
      </div>
      <ul class="p-3 space-y-3 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownCheckboxButton">
        <li>
          <div class="flex items-center">
            <input id="checkbox-item-1" type="checkbox" value="" class="cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded-sm focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
            <label for="checkbox-item-1" class=" cursor-pointer ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Africa</label>
          </div>
        </li>
        <li>
          <div class="flex items-center">
              <input checked id="checkbox-item-2" type="checkbox" value="" class="cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded-sm focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
              <label for="checkbox-item-2" class="cursor-pointer ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Americas</label>
            </div>
        </li>
        <li>
          <div class="flex items-center">
            <input id="checkbox-item-3" type="checkbox" value="" class="cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded-sm focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
            <label for="checkbox-item-3" class="cursor-pointer ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Antarctica</label>
          </div>
        </li>
        <li>
          <div class="flex items-center">
            <input id="checkbox-item-4" type="checkbox" value="" class="cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded-sm focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
            <label for="checkbox-item-4" class="cursor-pointer ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Asia</label>
          </div>
        </li>
        <li>
          <div class="flex items-center">
            <input id="checkbox-item-5" type="checkbox" value="" class="cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded-sm focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
            <label for="checkbox-item-5" class="cursor-pointer ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Europe</label>
          </div>
        </li>
        <li>
          <div class="flex items-center">
            <input id="checkbox-item-6" type="checkbox" value="" class="cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded-sm focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
            <label for="checkbox-item-6" class="cursor-pointer ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Oceania</label>
          </div>
        </li>
       
        <li>
          <div id="dateRangeDropdown" class="z-10 bg-white divide-y divide-gray-100 rounded-lg shadow-sm w-80 lg:w-96 dark:bg-gray-700 dark:divide-gray-600">
      <div class="p-3" aria-labelledby="dateRangeButton">
      <span class="text-gray-500 dark:text-gray-400">Population</span>
        <div date-rangepicker datepicker-autohide class="flex items-center gap-1">
            <div class="relative">
              <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
              </div>
              <label for="text" class="block pl-1 text-sm font-light text-gray-900 dark:text-white">min</label>
              <input name="min" type="number"  class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg 
         focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 
         block w-full p-2.5 
         dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white 
         dark:focus:ring-blue-500 dark:focus:border-blue-500"  placeholder="min">
            </div>
            <div class="relative">
              <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
              </div>
              <label for="text" class="block pl-1 text-sm font-light text-gray-900 dark:text-white">max</label>
              <input name="max" type="number"  class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg 
         focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 
         block w-full p-2.5 
         dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white 
         dark:focus:ring-blue-500 dark:focus:border-blue-500"  placeholder="max">
              </div>
        </div>
      </div>
      </div>
        </li>
      </ul>
  </div>
</div>
  </div>
</template>
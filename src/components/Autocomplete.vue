<script setup lang="ts">
import {dataService} from "../data.service";
import {ref} from "vue"

const open = ref(false)
const vModel = ref('')
const selected = ref({})
const jsonData = ref(dataService.retrieve())

const onType = (value: string): void => {
  jsonData.value = dataService.find(value)
  open.value = value != undefined && value != '' && jsonData.value.length > 0
}

const onClear = (): void => {
  selected.value = {}
  vModel.value = undefined
}

const onChange = (selection): void => {
  selected.value = selection
  vModel.value = `${selection.first_name} ${selection.last_name}`
  open.value = false
}

</script>

<template>
  <div class="flex-col flex">
    <!-- input -->
    <div class="flex items-center justify-between border border-gray-200 rounded-md px-2 py-1 bg-white">
      <input
        type="text"
        class="border-0 focus:outline-0 focus:ring-0 w-full py-1.5 text-sm text-gray-700 placeholder-neutral-600"
        autofocus
        autocomplete="off"
        v-model="vModel"
        @input="onType($event.target.value)"/>
      <button type="button"
              class="bg-gray-100 text-blue-600 font-medium text-[10px] border-0 rounded-md hover:bg-gray-200 py-1 px-1 uppercase"
              @click="onClear"
      >
        Clear
      </button>
    </div>

    <!-- results -->
    <div class="relative inline-block w-full mt-1">
      <ul
        :class="[open ? 'block opacity-100' : 'hidden opacity-0', 'list-none duration-150 transition max-h-52 overflow-y-auto absolute top-full border border-gray-200 shadow-sm rounded-md m-0 py-3 px-2 bg-white w-full' ]">
        <li
          v-for="record in jsonData"
          :class="[selected.id === record.id ? 'border-blue-200 border bg-blue-50' : undefined, 'hover:bg-blue-50 my-1 duration-200 cursor-pointer py-1.5 px-2 rounded-md' ]"
          @click="onChange(record)"
        >
          <div class="flex flex-col">
            <span class="text-sm text-gray-600">{{record.first_name}} {{record.last_name}}</span>
            <span class="text-neutral-600 text-xs">{{record.email}}</span>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>

</style>

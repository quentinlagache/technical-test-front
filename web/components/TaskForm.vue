<template>
  <form class="grid grid-cols-2 gap-6 px-10 py-8 bg-white shadow rounded-xl">
    <FormInput
      v-model="titleValue"
      class="col-span-2"
      name="task-title"
      label="Title"
    />
    <FormInputTextArea
      v-model="descriptionValue"
      class="col-span-2"
      name="task-description"
      label="Describe it"
    />
    <FormInput
      v-model="dateValue"
      type="datetime-local"
      name="task-date"
      label="Date"
    />
    <FormSelect
      v-model="statusValue"
      name="task-status"
      label="Status"
      :options="statusOptions"
    />
    <button
      type="submit"
      class="flex justify-center col-span-2 px-4 py-2 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      Submit
    </button>
    <NuxtLink
      :to="{ name: 'index' }"
      class="flex justify-center col-span-2 px-4 py-2 text-sm font-medium text-black border border-transparent rounded-md shadow-sm bg-gray-50 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-200"
    >
      Cancel
    </NuxtLink>
  </form>
</template>

<script>

import { TASK_STATUS } from '@/dewib/api/tasks'

export default {
  data () {
    return {
      titleValue: '',
      descriptionValue: '',
      dateValue: '',
      statusValue: '',
      statusOptions: [
        { value: '', label: 'Choose a status' },
        ...TASK_STATUS.filter(s => s.value !== 'ALL')
      ]
    }
  }
}
</script>

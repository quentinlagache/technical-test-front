<template>
  <form class="grid grid-cols-1 gap-6 px-10 py-8 bg-white shadow sm:grid-cols-2 sm:rounded-xl" @submit.prevent="submit">
    <FormInput
      v-model="titleValue"
      class="sm:col-span-2"
      name="task-title"
      label="Title"
      required
    />
    <FormInputTextArea
      v-model="descriptionValue"
      class="sm:col-span-2"
      name="task-description"
      label="Describe it"
      required
    />
    <FormInput
      v-model="dateValue"
      type="date"
      name="task-date"
      label="Date"
    />
    <FormSelect
      v-model="statusValue"
      name="task-status"
      label="Status"
      :options="statusOptions"
      required
    />
    <button
      type="submit"
      class="flex justify-center px-4 py-2 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md shadow-sm sm:col-span-2 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      Submit
    </button>
    <button
      type="button"
      class="flex justify-center px-4 py-2 text-sm font-medium text-black border border-transparent rounded-md shadow-sm sm:col-span-2 bg-gray-50 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-200"
      @click="$emit('cancel')"
    >
      Cancel
    </button>
  </form>
</template>

<script>

import { TASK_STATUS } from '@/dewib/api/tasks'

export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    description: {
      type: String,
      default: ''
    },
    status: {
      type: String,
      default: 'TODO',
      validator (value) {
        return TASK_STATUS.map(s => s.value).includes(value)
      }
    },
    date: {
      type: String,
      required: false,
      default: ''
    }
  },
  data () {
    return {
      titleValue: this.title,
      descriptionValue: this.description,
      dateValue: this.date,
      statusValue: this.status,
      statusOptions: [
        { value: '', label: 'Choose a status' },
        ...TASK_STATUS.filter(s => s.value !== 'ALL')
      ]
    }
  },
  methods: {
    submit () {
      this.$emit('submit', {
        title: this.titleValue,
        description: this.descriptionValue,
        ...(this.dateValue.length > 0 && { date: this.dateValue }),
        status: this.statusValue
      })
    }
  }
}
</script>

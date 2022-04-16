<template>
  <div class="px-4 py-4 sm:px-6 hover:bg-gray-50">
    <div class="flex justify-between">
      <div class="flex items-center flex-shrink overflow-hidden">
        <p class="inline-flex px-2 text-xs font-medium leading-5 rounded-full mr-2.5 flex-shrink-0" :class="{'text-red-800 bg-red-100' : status === 'TODO', 'text-green-800 bg-green-100': status === 'DONE'}">
          {{ statusLabel }}
        </p>
        <p class="text-sm font-medium text-indigo-600 truncate">
          {{ title }}
        </p>
      </div>
      <div class="flex items-center space-x-2.5 ml-5">
        <NuxtLink :to="{ name: 'tasks-id-edit', params: {id: id} }">
          <SvgIcon name="icons/pencil" class="w-5 h-5 p-1 text-white bg-indigo-600 rounded" />
        </NuxtLink>
        <button @click="$emit('remove')">
          <SvgIcon name="icons/trash" class="w-5 h-5 p-1 text-white bg-red-600 rounded" />
        </button>
      </div>
    </div>
    <div class="sm:flex sm:justify-between">
      <div v-if="date" class="flex items-center text-xs text-gray-500 sm:text-sm mt-2.5 sm:mt-2">
        <SvgIcon name="icons/calendar" class="w-4 h-4 mr-2 text-gray-400" aria-hidden="true" />
        <time :datetime="date">{{ dateLabel }}</time>
      </div>
      <p class="text-sm text-gray-500 sm:order-first mt-2.5 sm:mt-2" v-html="description" />
    </div>
    <div class="mt-2 sm:flex">
      <p class="text-xs text-gray-500 sm:mr-2">
        Created at
        {{ ' ' }}
        <time :datetime="createdAt">{{ createdAtLabel }}</time>
      </p>
      <p class="mt-2 text-xs font-bold text-gray-500 sm:mt-0">
        Updated at
        {{ ' ' }}
        <time :datetime="createdAt">{{ updatedAtLabel }}</time>
      </p>
    </div>
  </div>
</template>

<script>
import { TASK_STATUS } from '@/dewib/api/tasks'

export default {
  props: {
    id: {
      type: String,
      required: true
    },
    title: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    },
    status: {
      type: String,
      required: true,
      validator (value) {
        return TASK_STATUS.map(s => s.value).includes(value)
      }
    },
    statusLabel: {
      type: String,
      required: true,
      validator (value) {
        return TASK_STATUS.map(s => s.label).includes(value)
      }
    },
    date: {
      type: String,
      required: false,
      default: undefined
    },
    dateLabel: {
      type: String,
      required: false,
      default: undefined
    },
    createdAt: {
      type: String,
      required: true
    },
    updatedAt: {
      type: String,
      required: true
    },
    createdAtLabel: {
      type: String,
      required: true
    },
    updatedAtLabel: {
      type: String,
      required: true
    }
  }
}
</script>

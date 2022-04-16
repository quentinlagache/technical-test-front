<template>
  <div class="container m-10 mx-auto">
    <div class="flex items-center justify-between px-4 sm:px-0">
      <h1 class="text-lg font-medium text-gray-700">
        My tasks
      </h1>
      <FormSelect
        v-model="filter"
        name="filter"
        class="max-w-[170px]"
        :options="TASK_STATUS"
      />
    </div>
    <div class="w-full mt-5 border-t border-gray-300" />
    <div class="mt-8">
      <div v-if="$fetchState.pending">
        <div class="w-full bg-white divide-y divide-gray-200 shadow sm:rounded-xl">
          <div
            v-for="index in 3"
            :key="index"
            class="flex justify-between px-4 py-4 sm:px-6 animate-pulse"
            :style="{ animationDelay: index === 2 ? '600ms': index === 3 ? '1200ms': ''}"
          >
            <div class="w-full">
              <div class="flex space-x-4">
                <div class="w-20 h-5 bg-gray-200 rounded-full" />
                <div class="flex-1 max-w-sm py-1 space-y-6">
                  <div class="h-2 bg-gray-200 rounded" />
                </div>
              </div>
              <div class="max-w-sm mt-4 space-y-3">
                <div class="h-2 mr-8 bg-gray-200 rounded" />
                <div class="grid grid-cols-3 gap-4">
                  <div class="h-2 col-span-2 bg-gray-200 rounded" />
                  <div class="h-2 col-span-1 bg-gray-200 rounded" />
                </div>
              </div>
            </div>
            <div class="flex space-x-2.5">
              <div class="w-5 h-5 bg-gray-200 rounded" />
              <div class="w-5 h-5 bg-gray-200 rounded" />
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="$fetchState.error" class="p-4 rounded-md bg-red-50">
        <h3 class="text-sm font-medium text-red-800">
          Unable to fetch tasks
        </h3>
      </div>
      <div v-else-if="tasks.length > 0" class="overflow-hidden bg-white shadow sm:rounded-xl">
        <ul role="list" class="divide-y divide-gray-200">
          <li v-for="task in filteredTasks" :key="task.id">
            <TaskListItem
              :id="task.id"
              :status="task.status.value"
              :status-label="task.status.label"
              :title="task.title"
              :description="task.description"
              :date="task.date"
              :date-label="task.dateLabel"
              :created-at="task.createdAt"
              :created-at-label="task.createdAtLabel"
              :updated-at="task.updatedAt"
              :updated-at-label="task.updatedAtLabel"
            />
          </li>
        </ul>
      </div>
      <div v-else class="py-10 text-center">
        <h3 class="text-sm font-medium text-gray-900">
          No tasks
        </h3>
        <p class="mt-1 text-sm text-gray-500">
          Get started by creating a new task.
        </p>
        <div class="mt-6">
          <NuxtLink :to="{ name: 'add' }" class="inline-flex items-center px-4 py-2 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            <SvgIcon name="icons/plus-circle" class="w-4 h-4 mr-2" aria-hidden="true" />
            New Task
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { TASK_STATUS } from '~/dewib/api/tasks'

export default {
  data () {
    return {
      tasks: [],
      filter: 'ALL',
      TASK_STATUS
    }
  },
  async fetch () {
    // add fake timeout to show pending state
    // eslint-disable-next-line nuxt/no-timing-in-fetch-data
    await new Promise(resolve => setTimeout(resolve, 500))

    const [err, tasks] = await this.$api.tasks.findAll()

    if (err) {
      throw new Error(err)
    }

    this.tasks = tasks
  },
  computed: {
    filteredTasks () {
      if (this.filter === 'ALL') { return this.tasks }
      return this.tasks.filter(task => task.status.value === this.filter)
    }
  }
}
</script>

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
          <SkeletonTaskListItem
            v-for="index in 3"
            :key="index"
          />
        </div>
      </div>
      <Alert v-else-if="$fetchState.error">
        Unable to fetch tasks
      </Alert>
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
              @remove="removeTask(task.id)"
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
          <NuxtLink :to="{ name: 'tasks-add' }" class="inline-flex items-center px-4 py-2 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
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
    const queryParams = {}
    if (['DONE', 'TODO'].includes(this.filter)) { queryParams.status = this.filter }

    // add fake timeout to show pending state
    // eslint-disable-next-line nuxt/no-timing-in-fetch-data
    await new Promise(resolve => setTimeout(resolve, 500))

    const [err, tasks] = await this.$api.tasks.findAll(queryParams)

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
  },
  watch: {
    filter () {
      this.$fetch()
    }
  },
  methods: {
    async removeTask (id) {
      if (!confirm('Are you sure you want to delete this task?')) { return }
      const [err] = await this.$api.tasks.remove(id)

      if (err) {
        throw new Error(err)
      }

      this.tasks = this.tasks.filter(task => task.id !== id)
    }
  }
}
</script>

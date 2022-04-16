<template>
  <div class="max-w-md mx-auto">
    <div class="py-8 text-center">
      <SvgIcon name="icons/pencil" class="inline-block w-10 h-10" aria-hidden="true" />
      <h1 class="mt-6 text-3xl font-extrabold text-gray-900">
        Edit a task
      </h1>
    </div>
    <div v-if="$fetchState.pending" class="px-10 py-8 bg-white shadow rounded-xl">
      <div
        class="grid grid-cols-2 gap-6 animate-pulse"
      >
        <div class="w-full h-10 col-span-2 bg-gray-200 rounded" />
        <div class="w-full h-20 col-span-2 bg-gray-200 rounded" />
        <div class="w-full h-10 bg-gray-200 rounded" />
        <div class="w-full h-10 bg-gray-200 rounded" />
        <div class="w-full h-10 col-span-2 bg-gray-200 rounded" />
        <div class="w-full h-10 col-span-2 bg-gray-200 rounded" />
      </div>
    </div>
    <div v-else-if="$fetchState.error" class="p-4 rounded-md bg-red-50">
      <h3 class="text-sm font-medium text-red-800">
        Unable to fetch task {{ taskId }}
      </h3>
    </div>
    <TaskForm
      v-if="task"
      :title="task.title"
      :status="task.status.value"
      :description="task.description"
      :date="task.date"
      @submit="updateTask"
      @cancel="$router.push('/')"
    />
  </div>
</template>

<script>
export default {
  beforeRouteLeave (to, from, next) {
    if (!confirm('Are you sure you want to stop editing this task?')) { return false }
    next()
  },
  data () {
    return {
      taskId: this.$route.params.id,
      task: undefined
    }
  },
  async fetch () {
    // add fake timeout to show pending state
    // eslint-disable-next-line nuxt/no-timing-in-fetch-data
    await new Promise(resolve => setTimeout(resolve, 500))

    const [err, task] = await this.$api.tasks.findOne(this.taskId)

    if (err) {
      throw new Error(err)
    }

    this.task = task
  },
  methods: {
    async updateTask (form) {
      const [err] = await this.$api.tasks.update({ id: this.taskId, form })

      if (err) {
        throw new Error(err)
      }

      this.$router.push('/')
    }
  }
}
</script>

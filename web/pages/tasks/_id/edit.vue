<template>
  <div class="py-8 mx-auto space-y-8 sm:max-w-md">
    <div class="text-center">
      <SvgIcon name="icons/pencil" class="inline-block w-10 h-10" aria-hidden="true" />
      <h1 class="mt-6 text-3xl font-extrabold text-gray-900">
        Edit a task
      </h1>
    </div>
    <div v-if="$fetchState.pending" class="px-10 py-8 bg-white shadow rounded-xl">
      <SkeletonTaskForm />
    </div>
    <Alert v-else-if="$fetchState.error">
      Unable to fetch task
    </Alert>
    <Alert v-if="error">
      Unable to update task
    </Alert>
    <Alert v-if="success" type="success">
      Succesfully updated task
    </Alert>
    <TaskForm
      v-else-if="task"
      :title="task.title"
      :status="task.status.value"
      :description="task.description"
      :date="task.date"
      @submit="submit"
      @cancel="$router.push('/')"
    />
  </div>
</template>

<script>
export default {
  beforeRouteLeave (to, from, next) {
    if (this.task && !this.success && !confirm('Are you sure you want to stop editing this task?')) { return false }
    next()
  },
  data () {
    return {
      taskId: this.$route.params.id,
      task: undefined,
      success: false,
      error: false
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
    async submit (form) {
      this.success = false
      this.error = false
      try {
        await this.updateTask(form)

        this.success = true
        setTimeout(() => {
          this.$router.push('/')
        }, 1000)
      } catch (error) {
        this.error = true
      }
    },
    async updateTask (form) {
      this.success = false
      const [err] = await this.$api.tasks.update({ id: this.taskId, form })

      if (err) {
        throw new Error(err)
      }
    }
  }
}
</script>

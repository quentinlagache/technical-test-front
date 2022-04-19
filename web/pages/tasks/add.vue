<template>
  <div class="py-8 mx-auto space-y-8 sm:max-w-md">
    <div class="text-center">
      <SvgIcon name="icons/plus-circle" class="inline-block w-10 h-10" aria-hidden="true" />
      <h1 class="mt-6 text-3xl font-extrabold text-gray-900">
        Add a task
      </h1>
    </div>
    <Alert v-if="error">
      Unable to create task
    </Alert>
    <Alert v-if="success" type="success">
      Succesfully created task
    </Alert>
    <TaskForm
      v-else
      @submit="submit"
      @cancel="$router.push('/')"
    />
  </div>
</template>

<script>
export default {
  beforeRouteLeave (to, from, next) {
    if (!this.success && !confirm('Are you sure you want to stop editing this task?')) { return false }
    next()
  },
  data () {
    return {
      success: false,
      error: false
    }
  },
  methods: {
    async submit (form) {
      this.success = false
      this.error = false
      try {
        await this.createTask(form)

        this.success = true
        setTimeout(() => {
          this.$router.push('/')
        }, 1000)
      } catch (error) {
        this.error = true
      }
    },
    async createTask (form) {
      const [err] = await this.$api.tasks.create(form)

      if (err) {
        throw new Error(err)
      }
    }
  }
}
</script>

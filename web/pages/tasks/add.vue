<template>
  <div class="max-w-md mx-auto">
    <div class="py-8 text-center">
      <SvgIcon name="icons/plus-circle" class="inline-block w-10 h-10" aria-hidden="true" />
      <h1 class="mt-6 text-3xl font-extrabold text-gray-900">
        Add a task
      </h1>
    </div>
    <TaskForm
      @submit="createTask"
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
  methods: {
    async createTask (form) {
      const [err] = await this.$api.tasks.create(form)

      if (err) {
        throw new Error(err)
      }

      this.$router.push('/')
    }
  }
}
</script>

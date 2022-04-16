<template>
  <div class="py-8 mx-auto space-y-8 sm:max-w-md">
    <div class="text-center">
      <SvgIcon name="icons/plus-circle" class="inline-block w-10 h-10" aria-hidden="true" />
      <h1 class="mt-6 text-3xl font-extrabold text-gray-900">
        Add a task
      </h1>
    </div>
    <div v-if="success" class="p-4 bg-green-100 rounded-md">
      <h3 class="text-sm font-medium text-green-800">
        Succesfully created task
      </h3>
    </div>
    <TaskForm
      v-else
      @submit="createTask"
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
      success: false
    }
  },
  methods: {
    async createTask (form) {
      this.success = false
      const [err] = await this.$api.tasks.create(form)

      if (err) {
        throw new Error(err)
      }

      this.success = true
      setTimeout(() => {
        this.$router.push('/')
      }, 1000)
    }
  }
}
</script>

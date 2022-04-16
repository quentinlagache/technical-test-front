<template>
  <div>
    <label :for="name" class="block mb-1 text-sm font-medium text-gray-700">
      {{ label }}
    </label>
    <input
      :id="name"
      v-model="newValue"
      :type="type"
      :name="name"
      :required="required"
      class="block w-full px-3 py-2 placeholder-gray-400 border border-gray-300 rounded-md shadow-sm appearance-none focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
    >
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true
    },
    name: {
      type: String,
      required: true
    },
    value: {
      type: [String, Date],
      required: true
    },
    type: {
      type: String,
      default: 'text'
    },
    required: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      newValue: this.value
    }
  },
  watch: {
    value: 'onValueUpdated',
    newValue: 'onNewValueUpdated'
  },
  methods: {
    onValueUpdated () {
      this.newValue = this.value
    },
    onNewValueUpdated () {
      this.$emit('input', this.newValue)
    }
  }
}
</script>

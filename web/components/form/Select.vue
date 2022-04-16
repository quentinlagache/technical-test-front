<template>
  <div class="w-full">
    <label v-if="label" :for="name" class="block mb-1 text-sm font-medium text-gray-700">
      {{ label }}
    </label>
    <select v-model="newValue" :name="name" :required="required" class="block w-full border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
      <option v-for="option in options" :key="option.value" :value="option.value">
        {{ option.label }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    },
    label: {
      type: String,
      default: undefined
    },
    value: {
      type: String,
      required: true
    },
    options: {
      type: Array,
      required: true
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

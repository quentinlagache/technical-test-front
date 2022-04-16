<template>
  <div>
    <label :for="name" class="block mb-1 text-sm font-medium text-gray-700">
      {{ label }}
    </label>
    <textarea
      :id="name"
      v-model="newValue"
      :name="name"
      :required="required"
      rows="3"
      class="block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
    />
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
      type: String,
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

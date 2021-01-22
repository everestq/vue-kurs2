<template>
  <div class="form-control">
    <label :for="id">{{ title }}</label>
    <textarea
      :id="id"
      :rows="rows"
      @input="$emit('change-text', $event.target.value)"
      :value="value"></textarea>
  </div>
</template>

<script>
export default {
  emits: ['change-text'],
  props: {
    title: {
      type: String,
      required: true
    },
    id: {
      type: String,
      required: true
    },
    rows: {
      type: Number,
      required: false,
      default: 3
    },
    value: {
      type: String,
      required: false,
      default: ''
    }
  }
}
</script>

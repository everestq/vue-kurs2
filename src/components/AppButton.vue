<template>
  <button
    class="btn"
    :class="color"
    @click.prevent="$emit('action')"
    :disabled="!isActive"
  >{{ title }}</button>
</template>

<script>
export default {
  emits: ['action'],
  props: {
    title: {
      type: String,
      required: true
    },
    color: {
      type: String,
      required: false,
      default: ''
    },
    isActive: {
      type: Boolean,
      required: false,
      default: false
    }
  }
}
</script>

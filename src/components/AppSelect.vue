<template>
  <div class="form-control">
    <label for="type">{{ title }}</label>
    <select
      id="type"
      :value="currentOption"
      @change="$emit('change-type-block', $event.target.value)"
    >
      <option
        v-for="option in options"
        :key="option"
        :value="option"
      >{{ option }}</option>

    </select>
  </div>
</template>

<script>
export default {
  emits: ['change-type-block'],
  props: {
    title: {
      type: String,
      required: true
    },
    options: {
      type: Array,
      required: true
    },
    currentOption: {
      type: String,
      required: false
    }
  }
}
</script>

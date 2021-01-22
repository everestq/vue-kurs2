<template>
  <ul class="list">
    <li
      class="list-item"
      v-for="{id, email, body} in commentList"
      :key="id"
    >
      <div>
        <p><strong>{{ email }}</strong></p>
        <small>{{ body }}</small>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    commentList: {
      type: Array,
      required: true
    }
  }
}
</script>

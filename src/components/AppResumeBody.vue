<template>
  <div class="card card-w70">
    <div v-if="resumeContent.length">
      <component
        v-for="block in resumeContent"
        :key="block.id"
        :is="componentName(block.blockType)"
        v-bind="{ value: block.inputText }"
      ></component>
    </div>
    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>

</template>

<script>
import AppResumeTitle from '@/components/AppResumeTitle'
import AppResumeSubtitle from '@/components/AppResumeSubtitle'
import AppResumeAvatar from '@/components/AppResumeAvatar'
import AppResumeText from '@/components/AppResumeText'
export default {
  props: {
    resumeContent: {
      type: Array,
      required: true
    }
  },
  methods: {
    componentName (name) {
      if (name.toLowerCase() === 'заголовок') return 'app-resume-title'
      if (name.toLowerCase() === 'подзаголовок') return 'app-resume-subtitle'
      if (name.toLowerCase() === 'аватар') return 'app-resume-avatar'
      if (name.toLowerCase() === 'текст') return 'app-resume-text'
    }
  },
  components: {
    AppResumeTitle,
    AppResumeSubtitle,
    AppResumeAvatar,
    AppResumeText
  }
}
</script>

<style scoped>

</style>

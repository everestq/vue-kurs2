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
import ResumeTitle from '@/components/ResumeTitle'
import ResumeSubtitle from '@/components/ResumeSubtitle'
import ResumeAvatar from '@/components/ResumeAvatar'
import ResumeText from '@/components/ResumeText'
export default {
  props: {
    resumeContent: {
      type: Array,
      required: true
    }
  },
  methods: {
    componentName (name) {
      if (name.toLowerCase() === 'заголовок') return 'resume-title'
      if (name.toLowerCase() === 'подзаголовок') return 'resume-subtitle'
      if (name.toLowerCase() === 'аватар') return 'resume-avatar'
      if (name.toLowerCase() === 'текст') return 'resume-text'
    }
  },
  components: {
    ResumeTitle,
    ResumeSubtitle,
    ResumeAvatar,
    ResumeText
  }
}
</script>

<style scoped>

</style>

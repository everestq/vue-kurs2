<template>
  <form class="card card-w30">

    <app-select
      title="Тип блока"
      :options="options"
      :currentOption="blockType"
      @change-type-block="changeBlockType"
    ></app-select>

    <app-textarea
      title="Значение"
      id="value"
      @change-text="changeTextarea"
      :value="textarea"
    ></app-textarea>

    <app-button
      title="Добавить"
      color="primary"
      @action="addBlock"
      :is-active="isFormValid"
    ></app-button>
  </form>
</template>

<script>
import AppSelect from '@/components/AppSelect'
import AppTextarea from '@/components/AppTextarea'
import AppButton from '@/components/AppButton'

export default {
  emits: ['resume-block'],
  data () {
    return {
      textarea: '',
      blockType: 'Заголовок',
      options: ['Заголовок', 'Подзаголовок', 'Аватар', 'Текст']
    }
  },
  methods: {
    addBlock () {
      if (this.textarea.length > 3) {
        const obj = {
          id: new Date().getTime(),
          blockType: this.blockType,
          inputText: this.textarea
        }
        this.$emit('resume-block', obj)

        this.textarea = ''
        this.blockType = this.options[0]
      }
    },
    changeTextarea (value) {
      this.textarea = value
    },
    changeBlockType (value) {
      this.blockType = value
    }
  },
  computed: {
    isFormValid () {
      return this.textarea.length > 3
    }
  },
  components: {
    AppSelect,
    AppTextarea,
    AppButton
  }
}
</script>

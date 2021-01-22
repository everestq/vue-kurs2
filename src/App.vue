<template>
  <div class="container column">
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
        :is-active="activeBtn"
      ></app-button>
    </form>

    <app-resume-body
      v-if="!loadingResume"
      :resumeContent="resumeContent"
    ></app-resume-body>
  </div>
  <div class="container">
    <p v-if="!isCommentLoaded">
      <app-button
        title="Загрузить комментарии"
        color="primary"
        :is-active="true"
        @action="loadComment"
      ></app-button>
    </p>
    <div class="card" v-else>
      <h2>Комментарии</h2>
      <app-comment-list
        :comment-list="commentList"
      ></app-comment-list>
    </div>
    <app-loader v-if="loading"></app-loader>
  </div>
</template>

<script>
import './theme.css'
import AppSelect from '@/components/AppSelect'
import AppTextarea from '@/components/AppTextarea'
import AppButton from '@/components/AppButton'
import AppResumeBody from '@/components/AppResumeBody'
import AppLoader from '@/components/AppLoader'
import AppCommentList from '@/components/AppCommentList'

import axios from 'axios'

export default {
  data () {
    return {
      isCommentLoaded: false,
      loading: false,
      resumeContent: [],
      commentList: [],
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
        this.resumeContent.push(obj)
        axios.post('https://vue-with-http-60378-default-rtdb.firebaseio.com/resume1.json', obj)
          .then(function (response) {
            console.log(response)
          })
          .catch(function (error) {
            console.log(error)
          })

        this.textarea = ''
        this.blockType = this.options[0]
      }
    },
    changeTextarea (value) {
      this.textarea = value
    },
    changeBlockType (value) {
      this.blockType = value
    },
    async loadComment () {
      try {
        this.loading = true

        const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.commentList = data
        this.isCommentLoaded = true
        if (!data) {
          throw new Error('Список комментариев пуст')
        }

        this.loading = false
      } catch (e) {
        console.log(e.message)
        this.loading = false
      }
    },
    async loadResume () {
      try {
        const { data } = await axios.get('https://vue-with-http-60378-default-rtdb.firebaseio.com/resume1.json')
        if (!data) {
          throw new Error('Список людей пуст')
        }
        this.resumeContent = Object.keys(data).map(key => {
          return {
            ...data[key]
          }
        })
      } catch (e) {
        console.log(e.message)
      }
    }
  },
  mounted () {
    this.loadResume()
  },
  computed: {
    activeBtn () {
      return this.textarea.length > 3
    }
  },
  components: {
    AppSelect,
    AppTextarea,
    AppButton,
    AppResumeBody,
    AppLoader,
    AppCommentList
  }
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>

<template>
  <div class="container column">

    <app-form
      @resume-block="addBlock"
    ></app-form>
    <app-resume-body
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
import AppForm from '@/components/AppForm'
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
      commentList: []
    }
  },
  methods: {
    addBlock (obj) {
      axios.post('https://vue-with-http-60378-default-rtdb.firebaseio.com/resume1.json', obj)
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          console.log(error)
        })
      this.resumeContent.push(obj)
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
  components: {
    AppForm,
    AppButton,
    AppResumeBody,
    AppLoader,
    AppCommentList
  }
}
</script>

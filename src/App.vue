<template>
  <div class="container column">
    <resume-form @block-added="createBlock"></resume-form>
    <resume-view :blocks="items"></resume-view>
  </div>
  <div class="container">
    <resume-comments v-if="!loading"
      :comments="commentsList"
      @load="loadComments"
    ></resume-comments>
    <app-loader v-else></app-loader>
  </div>
</template>

<script>
import axios from 'axios'
import ResumeForm from './components/ResumeForm'
import ResumeView from './components/ResumeView'
import ResumeComments from './components/ResumeComments'
import AppLoader from './components/AppLoader'

export default {
  components: {
    ResumeComments, AppLoader, ResumeForm, ResumeView
  },
  mounted () {
    this.loadBlocks()
  },
  data () {
    return {
      items: [],
      commentsList: [],
      loading: false
    }
  },
  methods: {
    async createBlock (block) {
      const response = await axios.post('https://vue-resume2-default-rtdb.firebaseio.com/blocks.json',
        {
          dataBlockType: block.type,
          dataBlockText: block.content
        })
      const { data } = await response
      this.items.push({
        id: data.name,
        type: block.type,
        content: block.content
      })
    },
    async loadBlocks () {
      try {
        const { data } = await axios.get('https://vue-resume2-default-rtdb.firebaseio.com/blocks.json')
        if (!data) {
          throw new Error('Записей нет')
        }
        this.items = Object.keys(data).map(key => {
          return {
            id: key,
            content: data[key].dataBlockText,
            type: data[key].dataBlockType
          }
        })
      } catch (e) {
        e.message()
      }
    },
    async loadComments () {
      try {
        this.loading = true
        const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        if (!data) {
          this.loading = false
          throw new Error('Список комментариев пуст')
        }
        this.commentsList = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loading = false
      } catch (e) {
        e.message()
      }
    }
  }
}
</script>

<style>

</style>

<template>
  <div class="container column">
    <app-form @block-added="createBlock"></app-form>
    <app-resume :blocks="items"></app-resume>
  </div>
  <div class="container">
    <app-comments-list v-if="!loading"
      :comments="commentsList"
      @load="loadComments"
    ></app-comments-list>
    <app-loader v-else></app-loader>
  </div>
</template>

<script>
import axios from 'axios'
import AppForm from './components/AppForm'
import AppResume from './components/AppResume'
import AppCommentsList from './components/AppCommentsList'
import AppLoader from './components/AppLoader'

export default {
  components: {
    AppCommentsList, AppLoader, AppForm, AppResume
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

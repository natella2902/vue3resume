<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="createBlock">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="blockType">
          <option value="title" >Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" v-model.trim="blockText" rows="3"></textarea>
      </div>

      <button class="btn primary" :disabled="disabledBtn">Добавить</button>
    </form>
    <div class="card card-w70">
      <div v-if="items.length">
          <component v-for="item in items"
                     :key="item.id"
                     :is="item.block"
                     :text="item.content">
          </component>
      </div>
      <div v-else>
        <h3>Добавьте первый блок, чтобы увидеть результат</h3>
      </div>
    </div>
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
import AppTitle from '@/AppTitle'
import AppSubtitle from '@/AppSubtitle'
import AppAvatar from '@/AppAvatar'
import AppText from '@/AppText'
import AppCommentsList from '@/AppCommentsList'
import AppLoader from './AppLoader'

export default {
  components: {
    AppTitle, AppSubtitle, AppAvatar, AppText, AppCommentsList, AppLoader
  },
  data () {
    return {
      blockText: '',
      blockType: 'title',
      items: [],
      commentsList: [],
      loading: false
    }
  },
  mounted () {
    this.loadBlocks()
  },
  computed: {
    disabledBtn () {
      return this.blockText.length <= 3
    }
  },
  methods: {
    async createBlock () {
      const response = await axios.post('https://vue-resume2-default-rtdb.firebaseio.com/blocks.json',
        {
          dataBlockType: this.blockType,
          dataBlockText: this.blockText
        })
      const { data } = await response
      this.items.push({
        id: data.name,
        block: 'app-' + this.blockType,
        content: this.blockText
      })
      this.blockText = ''
      this.blockType = 'title'
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
            block: 'app-' + data[key].dataBlockType
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

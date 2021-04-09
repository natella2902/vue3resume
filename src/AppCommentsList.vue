<template>
  <p v-if="!comments.length">
    <app-btn @load="$emit('load')">Nene</app-btn>
  </p>
  <div class="card" v-else>
    <h2>Комментарии</h2>
    <ul class="list" v-for="comment in comments" :key="comment.id">
      <li class="list-item">
        <div>
          <p><strong>{{ comment.email }}</strong></p>
          <small>{{comment.body }}</small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import AppBtn from '@/AppBtn'
export default {
  components: { AppBtn },
  emits: {
    load: null
  },
  props: {
    comments: {
      type: Array,
      required: false
    }
  },
  methods: {
    loadList () {
      console.log('click')
    }
  }
}
</script>

<style scoped>

</style>

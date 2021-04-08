<template>
  <p v-if="!comments.length">
    <button
      class="btn primary"
      @click="$emit('load')"
    >Загрузить комментарии</button>
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
export default {
  emits: ['load'],
  props: {
    comments: Array
  }
}
</script>

<style scoped>

</style>

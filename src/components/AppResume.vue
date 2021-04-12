<template>
  <div class="card card-w70">
    <div v-if="blocks.length">
      <component v-for="block in blocks"
                 :key="block.id"
                 :is="block.type"
                 v-bind="{value: block.content}"
      >
      </component>
    </div>
    <div v-else>
      <h3>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
</template>

<script>
import AppTitle from './parts/ResumeTitle'
import AppSubtitle from './parts/ResumeSubtitle'
import AppAvatar from './parts/ResumeAvatar'
import AppText from './parts/ResumeText'

export default {
  components: {
    AppTitle, AppSubtitle, AppAvatar, AppText
  },
  props: {
    blocks: {
      type: Array
    }
  }
}
</script>

<style scoped>

</style>

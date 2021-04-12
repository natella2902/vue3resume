<template>
  <button
    class="btn primary"
    @click="$emit('load')"
  >Загрузить комментарии</button>
</template>

<script>
export default {
  emits: {
    load: null
  }
}
</script>

<style scoped>

</style>

<template>
  <h1 class="red">
    {{ text }}
  </h1>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      required: true,
      validator (value) {
        return typeof value === 'string'
      }
    }
  }
}
</script>

<style scoped>

</style>

<template>
<h2>
  {{ text }}
</h2>
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

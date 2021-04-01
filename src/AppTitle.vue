<template>
  <h1 class="red">
    {{ title.title }}
  </h1>
</template>

<script>
export default {
  props: {
    title: String
  }

}
</script>

<style scoped>
.red {
  color: #c25205;
}
</style>

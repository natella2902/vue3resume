<template>
  <div class="avatar">
    <img :src="text">
  </div>
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

.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
}

</style>

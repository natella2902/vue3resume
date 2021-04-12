<template>
  <div class="avatar">
    <img :src="$attrs.value">
  </div>
</template>

<script>

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

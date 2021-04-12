<template>
  <form class="card card-w30" @submit.prevent="submit">
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

    <button class="btn primary" :disabled="isValidForm">Добавить</button>
  </form>
</template>

<script>

export default {
  data () {
    return {
      blockType: 'title',
      blockText: ''
    }
  },
  computed: {
    isValidForm () {
      return this.blockText.length <= 3
    }
  },
  methods: {
    async submit () {
      this.$emit('block-added', {
        type: 'app-' + this.blockType,
        content: this.blockText
      })
      this.blockText = ''
      this.blockType = 'title'
    }
  }
}
</script>

<style scoped>

</style>

<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="createBlock">
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
        <textarea id="value" v-model="text" rows="3"></textarea>
      </div>

      <button class="btn primary" :disabled="text.length <= 3">Добавить</button>
    </form>

    <div class="card card-w70">
      <app-title
        v-for="block in blocks"
        :key="block"
        :title="block"
      >
      </app-title>
      <app-subtitle
        v-for="block in blocks"
        :key="block"
        :title="block"
      >
      </app-subtitle>
      <h1>Резюме Nickname</h1>
      <div class="avatar">
        <img src="https://cdn.dribbble.com/users/5592443/screenshots/14279501/drbl_pop_r_m_rick_4x.png">
      </div>
      <h2>Опыт работы</h2>
      <p>
        главный герой американского мультсериала «Рик и Морти», гениальный учёный, изобретатель, атеист (хотя в некоторых сериях он даже молится Богу, однако, каждый раз после чудесного спасения ссылается на удачу и вновь отвергает его существование), алкоголик, социопат, дедушка Морти. На момент начала третьего сезона ему 70 лет[1]. Рик боится пиратов, а его главной слабостью является некий - "Санчезиум". Исходя из того, что существует неограниченное количество вселенных, существует неограниченное количество Риков, герой сериала предположительно принадлежит к измерению С-137. В серии комикcов Рик относится к измерению C-132, а в игре «Pocket Mortys» — к измерению C-123[2]. Прототипом Рика Санчеза является Эмметт Браун, герой кинотрилогии «Назад в будущее»[3].
      </p>
      <h3>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <div class="container">
    <p>
      <button class="btn primary">Загрузить комментарии</button>
    </p>
    <div class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item">
          <div>
            <p><strong>test@microsoft.com</strong></p>
            <small>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Eligendi, reiciendis.</small>
          </div>
        </li>
      </ul>
    </div>
    <div class="loader"></div>
  </div>
</template>

<script>
import axios from 'axios'
import AppTitle from '@/AppTitle'
import AppSubtitle from '@/AppSubtitle'
export default {
  data () {
    return {
      text: '',
      blockType: 'title',
      blocks: []
    }
  },
  components: {
    AppTitle, AppSubtitle
  },
  methods: {
    async createBlock () {
      console.log(this.blockType, this.text)
      const response = await axios.post('https://vue-resume-e0e35-default-rtdb.firebaseio.com/blocks.json',
        { blockText: this.text })

      const { data } = await response

      console.log(data.name.title)

      this.blocks.push({
        title: this.text
      })
      this.text = ''
    }
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>

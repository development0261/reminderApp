<template>
  <div class="main-layout">
    <div class="back-to-list">
      <a href="/">
        <img src="../static/images/left.svg">
        List
      </a>
    </div>
    <h2 class="list-view-title">
      Reminder
    </h2>
    <div class="list-view">
      <ul>
        <li v-for="(list, index) in results" :key="index">
          <h6>{{list.title}}</h6>
          <p>{{list.description}}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import '../static/css/style.css'
export default {
  name: 'ListsPage',
  data () {
    return {
      results: []
    }
  },
  async mounted () {
    await this.$axios.get(process.env.NUXT_ENV_API_URL + '/reminders').then((response) => { this.results = response.data.data })
  }
}
</script>

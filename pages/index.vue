<template>
  <div class="main-layout">
    <div class="search-box">
      <SearchBox />
    </div>
    <div class="reminder-list">
      <MyList :count="results" />
    </div>
    <button class="new-reminder" @click="modalactive = !modalactive">
      <img src="../static/images/plus-circle.svg">
      New Reminder
    </button>
    <div class="new-reminder-modal" :class="{ 'modal-active': modalactive }">
      <ReminderModal @close="modalactive = !modalactive" @reminders="getReminders" />
    </div>
  </div>
</template>

<script>
import '../static/css/style.css'
export default {
  name: 'IndexPage',
  data () {
    return {
      modalactive: false,
      listActive: false,
      results: 0
    }
  },
  // watch: {
  //   results (v) {
  //     console.log('results has been updated', v)
  //     // this.getReminders()
  //   }
  // },
  mounted () {
    this.getReminders()
  },
  methods: {
    async getReminders () {
      await this.$axios.get(process.env.NUXT_ENV_API_URL + '/reminders').then((response) => { this.results = response.data.count })
    }
  }
}
</script>

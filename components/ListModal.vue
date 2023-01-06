<template>
  <div class="new-list-modal-box">
    <div class="reminder-header" :class="{ 'inner-header': changeslide }">
      <button class="cancle-reminder" @click="hideModal">
        <img src="../static/images/left.svg">
        List
      </button>
      <h4 class="details-title">
        Details
      </h4>
      <button class="back-reminder" @click="changeslide = !changeslide">
        <img src="../static/images/left.svg">
        List Back
      </button>
    </div>
    <div class="reminder-slides" :class="{ 'change-slide': changeslide }">
      <div class="new-reminder-slide">
        <div class="list-view">
          <ul>
            <li v-for="(list, index) in results" :key="index" @click="openModal(list)">
              <h6>{{ list.title }}</h6>
              <p>{{ list.description }}</p>
            </li>
          </ul>
        </div>
      </div>
      <div v-if="modalVisible" class="details-slide">
        <h6>{{ modalData.title }}</h6>
        <p>{{ modalData.description }}</p>
        <p>{{ modalData.due_date + modalData.due_time }}</p>
        <p>
          <img v-for="(file, index) in JSON.parse(modalData.attachment)" :key="index" :src="url + '/' + file" style="height: 130px;width: 25%;margin-left: 10px;">&nbsp;
        </p>
      </div>
    </div>
  </div>
</template>
<script>
// import ReminderDetails from './ReminderDetails'
export default {
  name: 'ListModal',
  components: {
    // ReminderDetails
  },
  data () {
    return {
      changeslide: false,
      results: [],
      modalVisible: false,
      modalData: null,
      url: process.env.NUXT_ENV_ATTACHMENT_URL
    }
  },
  async mounted () {
    await this.$axios.get(process.env.NUXT_ENV_API_URL + '/reminders').then((response) => { this.results = response.data.data })
  },
  methods: {
    hideModal () {
      this.$emit('close')
    },
    openModal (data) {
      this.modalData = data
      this.modalVisible = true
      this.changeslide = !this.changeslide
    }
  }
}
</script>

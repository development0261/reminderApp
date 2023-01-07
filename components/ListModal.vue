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
    <div class="reminder-slides reminders-main-slide" :class="{ 'change-slide': changeslide }">
      <div class="new-reminder-slide ">
        <h4 class="reminders-main-title">Reminders</h4>
        <div class="list-view">
          <ul>
            <li v-for="(list, index) in data" :key="index" @click="openModal(list)">
              <h6>{{ (list.title.length > 0) ? list.title : '' }}</h6>
              <p>{{ (list.description != '' ) ? list.description : '' }}</p>
              <p class="due-date-time">{{ (list.due_date != '' ) ? list.due_date : '' }}, {{ (list.due_time != '' ) ? list.due_time : '' }}</p>
              <div class="uploded-image-list" v-if="JSON.parse(list.attachment).length > 0">
                <div v-for="(file, fileIndex) in JSON.parse(list.attachment)" :key="fileIndex" class="uploded-image-box">
                  <img :src="url + '/' + file">
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div v-if="modalVisible" class="details-slide">
        <h6 class="reminder-detail-title">{{ modalData.title }}</h6>
        <p class="reminder-detail-text">{{ modalData.description }}</p>
        <p  class="due-date-time">{{ modalData.due_date + modalData.due_time }}</p>
        <div class="reminder-detail-images">
          <img v-for="(file, index) in JSON.parse(modalData.attachment)" :key="index" :src="url + '/' + file">&nbsp;
        </div>
      </div>
    </div>
  </div>
</template>
<script>

export default {
  name: 'ListModal',
  props: {
    data: {
      type: Array,
      default: () => []
    }
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

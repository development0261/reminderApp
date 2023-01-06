<!-- Please remove this file from your project -->
<template>
  <div class="new-reminder-modal-box">
    <div class="reminder-header" :class="{ 'inner-header': changeslide }">
      <button class="cancle-reminder" @click="hideModal">
        Cancel
      </button>
      <button class="back-reminder" @click="changeslide = !changeslide">
        <img src="../static/images/left.svg">
        New Reminder
      </button>
      <h4 class="new-reminder-title">
        New Reminder
      </h4>
      <h4 class="details-title">
        Details
      </h4>
      <button class="add-reminder" @click="formSubmit">
        <!-- add-reminder-disable -->
        Add
      </button>
    </div>
    <div class="reminder-slides" :class="{ 'change-slide': changeslide }">
      <div class="new-reminder-slide">
        <div class="new-reminder-titles">
          <input v-model="title" type="text" placeholder="Title">
          <textarea v-model="description" rows="4" placeholder="Notes" />
        </div>
        <button class="reminder-details-btn" @click="changeslide = !changeslide">
          Details<img src="../static/images/right.svg">
        </button>
        <div class="add-image-box">
          <b-dropdown text="Add Image">
            <b-dropdown-item-button>
              <label for="take-photo">
                <input id="take-photo" ref="file1" type="file" multiple @change="handleFileUpload($event)">
                <p>Take Photo</p>
                <img src="../static/images/camera.svg">
              </label>
            </b-dropdown-item-button>
            <b-dropdown-item-button>
              <label for="take-photo">
                <input id="take-photo" ref="file2" type="file" @change="handleFileUpload($event)">
                <p>Scan Document</p>
                <img src="../static/images/scan.svg">
              </label>
            </b-dropdown-item-button>
            <b-dropdown-item-button>
              <label for="take-photo">
                <input id="take-photo" ref="file3" type="file" @change="handleFileUpload($event)">
                <p>Scan Document</p>
                <img src="../static/images/scan.svg">
              </label>
            </b-dropdown-item-button>
          </b-dropdown>
          <div class="add-image-list">
            <ul>
              <li v-for="(file, index) in url" :key="index">
                <!-- ask-delete -->
                <div class="upload-image-box">
                  <span>
                    <img src="../static/images/minus-circle.svg">
                  </span>
                  <img :src="file">
                  <h6>Image</h6>
                </div>
                <button class="delete-upload">
                  Delete
                </button>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div class="details-slide">
        <div class="date-time-box">
          <ul>
            <li>
              <div class="date-time-btn-outer">
                <label class="date-time-switch">
                  <input type="checkbox">
                  <span class="date-time-switch-slider" />
                </label>
                <b-button v-b-toggle.collapse-1 class="date-time-btn">
                  <span class="date-time-icon date-icon">
                    <img src="../static/images/calendar.svg">
                  </span>
                  <div class="date-time-text">
                    <h6>Date</h6>
                    <p>
                      <client-only> <vue-datepicker class="datePicker" reference="menu" v-model="date_today" :config="{collapse:true}" @click="openDatepicker" /> </client-only>
                    </p>
                  </div>
                </b-button>
              </div>
              <b-collapse id="collapse-1">
                <div class="date-time-details">
                  <!-- <client-only> <vue-datepicker class="fileInput" reference="menu" v-model="date_today" :config="{collapse:true}" /> </client-only> -->
                </div>
              </b-collapse>
            </li>
            <li>
              <div class="date-time-btn-outer">
                <label class="date-time-switch">
                  <input type="checkbox">
                  <span class="date-time-switch-slider" />
                </label>
                <b-button v-b-toggle.collapse-2 class="date-time-btn">
                  <span class="date-time-icon time-icon">
                    <img src="../static/images/time.svg">
                  </span>
                  <div class="date-time-text">
                    <h6>Time</h6>
                    <!-- <p>14:00</p> -->
                    <client-only> <vue-datepicker class="timePicker" reference="menu" v-model="time_today" :config="{collapse:true}" /> </client-only>
                  </div>
                </b-button>
              </div>
              <b-collapse id="collapse-2">
                <div class="date-time-details1">
                  <h2>test</h2>
                </div>
              </b-collapse>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'ReminderModal',
  data () {
    return {
      changeslide: false,
      openpicker: false,
      date_today: new Date(),
      time_today: new Date(),
      title: '',
      description: '',
      due_time: new Date(),
      attachment: null,
      url: []
    }
  },
  methods: {
    hideModal () {
      this.$emit('close')
    },
    handleFileUpload () {
      this.attachment = this.$refs.file1.files
      for (let i = 0; i < this.attachment.length; i++) {
        this.url[i] = URL.createObjectURL(this.attachment[i])
        // this.$store.commit('addFiles', URL.createObjectURL(this.attachment[i]))
      }
    },
    async formSubmit () {
      const data = new FormData()
      data.append('title', this.title)
      data.append('description', this.description)
      data.append('due_date', this.date_today)
      data.append('due_time', this.due_time)
      for (let i = 0; i < this.attachment.length; i++) {
        data.append('attachment[' + i + ']', this.$refs.file1.files[i])
      }

      const config = {
        header: {
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Methods': 'GET, POST, PATCH, PUT, DELETE, OPTIONS',
          'Access-Control-Allow-Headers': 'Origin, Content-Type, X-Auth-Token',
          'Content-Type': 'multipart/form-data'
        }
      }
      await this.$axios.post(process.env.NUXT_ENV_API_URL + '/reminders', data, config).then((response) => {
        if (response.data.status === 200) {
          this.$emit('reminders')
          alert(response.data.msg)
          this.hideModal()
        } else {
          alert(response.data.msg)
        }
      }).catch((err) => {
        alert(err)
      })
    },
    openDatepicker () {
      this.openpicker = !this.openpicker
      if (this.openpicker) {
        // document.getElementsByClassName('datePicker')[0].focus()
        document.getElementsByClassName('bootstrap-datetimepicker-widget')[0].remove()
      }
      // setTimeout(() => {
      //   const elementExists = document.getElementsByClassName('bootstrap-datetimepicker-widget')[0]
      //   console.log(typeof (elementExists), elementExists)
      //   // && elementExists != null
      //   if (typeof (elementExists) === 'undefined') {
      //     console.log('Hi')
      //   } else {
      //     document.getElementsByClassName('fileInput')[0].focus()
      //     console.log('bye')
      //   }
      //   // if (fileUpload != null) {
      //   //   fileUpload.click()
      //   // }
      // }, 500)
    }
  }
}
</script>

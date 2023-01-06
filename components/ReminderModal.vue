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
      <button class="add-reminder" :class=" (enableSubmit) ? '' : 'add-reminder-disable' " @click="formSubmit">
        Add
      </button>
    </div>
    <div class="reminder-slides" :class="{ 'change-slide': changeslide }">
      <div class="new-reminder-slide">
        <div class="new-reminder-titles">
          <input v-model="title" type="text" placeholder="Title">
          <span v-if="isError" class="titleError">Please fill this field.</span>
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
                <p>Photo Library</p>
                <img src="../static/images/images.svg">
              </label>
            </b-dropdown-item-button>
          </b-dropdown>
          <div class="add-image-list">
            <ul>
              <li v-for="(file, index) in filesdata" :key="index" :class="(slideDelete && index == fileIndex) ? 'ask-delete' : ''" @click="openDelete(index)">
                <div class="upload-image-box">
                  <span>
                    <img src="../static/images/minus-circle.svg">
                  </span>
                  <img :src="file">
                  <h6>Image</h6>
                </div>
                <button class="delete-upload" @click="deleteEvent(index)">
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
                <b-button class="date-time-btn" v-b-toggle.collapse-1>
                  <span class="date-time-icon date-icon">
                    <img src="../static/images/calendar.svg">
                  </span>
                  <div class="date-time-text">
                    <h6>Date</h6>
                    <p>
                      {{ selectedDate }}
                    </p>
                  </div>
                </b-button>
              </div>
              <b-collapse id="collapse-1">
                <div class="date-time-details">
                  <client-only>
                    <vue-datepicker v-model="date_today" :inline="true" @selected="datePickerEvent" />
                  </client-only>
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
                    <p>
                      Current
                    </p>
                    <!-- <p>14:00</p> -->
                    <!-- <client-only> <vue-datepicker class="timePicker" reference="menu" v-model="time_today" :config="{collapse:true}" /> </client-only> -->
                  </div>
                </b-button>
              </div>
              <b-collapse id="collapse-2">
                <div class="date-time-details1">
                  <vue-timepicker v-model="yourTimeValue" format="HH:mm:ss" />
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
      askDelete: false,
      openpicker: false,
      date_today: new Date(),
      selectedDate: '',
      time_today: new Date(),
      title: '',
      description: '',
      due_time: new Date(),
      attachment: [],
      url: [],
      filesdata: [],
      enableSubmit: false,
      isError: false,
      slideDelete: false,
      fileIndex: '',
      yourTimeValue: {
        HH: '10',
        mm: '05',
        ss: '00'
      }
    }
  },
  watch: {
    title (value) {
      this.title = value
      if (value === '') {
        this.enableSubmit = false
        this.isError = true
      } else {
        this.enableSubmit = true
        this.isError = false
      }
    }
  },
  mounted () {
    this.selDel()
  },
  methods: {
    selDel () {
      const dt = new Date(this.date_today)
      this.selectedDate = dt.getDate() + '/' + dt.getMonth() + 1 + '/' + dt.getFullYear()
    },
    datePickerEvent (event) {
      this.date_today = event
      const dt = new Date(this.date_today)
      this.selectedDate = dt.getDate() + '/' + (dt.getMonth() + 1) + '/' + dt.getFullYear()
    },
    hideModal () {
      this.$emit('close')
    },
    handleFileUpload () {
      this.attachment = this.$refs.file1.files
      for (let i = 0; i < this.attachment.length; i++) {
        this.url = URL.createObjectURL(this.attachment[i])
        this.filesdata.push(this.url)
      }
    },
    async formSubmit () {
      const dateStr = this.date_today
      const data = new FormData()
      data.append('title', this.title)
      data.append('description', this.description)
      data.append('due_date', new Date(dateStr).toISOString())
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
    deleteEvent (event) {
      this.filesdata.splice(event, 1)
    },
    openDelete (index) {
      this.slideDelete = !this.slideDelete
      this.fileIndex = index
    }
  }
}
</script>

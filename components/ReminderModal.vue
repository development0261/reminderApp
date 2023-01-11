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
          <input v-model="title" type="text" placeholder="Title" @blur="validateTitle($event)">
          <span v-if="isError" class="titleError">Please fill this field.</span>
          <textarea v-model="description" rows="4" placeholder="Notes" />
        </div>
        <button class="reminder-details-btn" @click="changeslide = !changeslide">
          Details<img src="../static/images/right.svg">
        </button>
        <div class="add-image-box">
          <b-dropdown text="Add Image">
            <b-dropdown-item-button>
              <label @click="toggleCamera">
                <!-- for="take-photo" <input id="take-photo" type="file" accept="image/*;capture=camera"> -->
                <p>Take Photo</p>
                <img src="../static/images/camera.svg">
              </label>
            </b-dropdown-item-button>
            <b-dropdown-item-button>
              <label for="photo-library">
                <input id="photo-library" ref="file1" type="file" multiple @change="handleFileUpload($event)">
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
          <div v-show="isCameraOpen && isLoading" class="camera-loading">
            <ul class="loader-circle">
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <div v-if="isCameraOpen" v-show="!isLoading" class="camera-box" :class="{ 'flash' : isShotPhoto }">
            <div class="camera-shutter" :class="{'flash' : isShotPhoto}"></div>
            <video v-show="!isPhotoTaken" ref="camera" :width="450" :height="337.5" autoplay></video>
            <canvas v-show="isPhotoTaken" id="photoTaken" ref="canvas" :width="450" :height="337.5"></canvas>
          </div>
          <div v-if="isCameraOpen && !isLoading" class="camera-shoot">
            <a id="javascript:void(0)" class="button" role="button" @click="toggleCamera">
              Cancel
            </a>
            <button type="button" class="button" @click="takePhoto">
              <img src="https://img.icons8.com/material-outlined/50/000000/camera--v2.png">
            </button>
          </div>
          <div v-if="isPhotoTaken && isCameraOpen" class="camera-download">
            <a id="retakePhoto" class="button" role="button" @click="takePhoto">
              Retake
            </a>
            <a id="downloadPhoto" class="button" role="button" @click="downloadImage">
              Use Photo
            </a>
          </div>
        </div>
      </div>
      <div class="details-slide">
        <div class="date-time-box">
          <ul>
            <li>
              <div class="date-time-btn-outer">
                <label class="date-time-switch">
                  <input type="checkbox" @click="switchChecked($event, 'datepicker')">
                  <span class="date-time-switch-slider" />
                </label>
                <b-button id="clDatepicker" class="date-time-btn " :class="(isDateCollapse && switchCheckedBool) ? 'not-collapsed' : 'collapsed'" @click="collapsePicker('datepicker')">
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
              <div id="openDatepicker" :class="(isDateCollapse && switchCheckedBool) ? 'collapse show' : 'collapse'">
                <div class="date-time-details">
                  <client-only>
                    <vue-datepicker v-model="date_today" :inline="true" @selected="datePickerEvent" />
                  </client-only>
                </div>
              </div>
            </li>
            <li>
              <div class="date-time-btn-outer">
                <label class="date-time-switch">
                  <input type="checkbox" @click="switchChecked($event, 'timepicker')">
                  <span class="date-time-switch-slider" />
                </label>
                <b-button id="clTimepicker" class="date-time-btn" :class="(isTimeCollapse && timeSwitchCheckedBool) ? 'not-collapsed' : 'collapsed'" @click="collapsePicker('timepicker')">
                  <span class="date-time-icon time-icon">
                    <img src="../static/images/time.svg">
                  </span>
                  <div class="date-time-text">
                    <h6>Time</h6>
                    <p>
                      {{ selectedTime }}
                    </p>
                    <!-- <p>14:00</p> -->
                    <!-- <client-only> <vue-datepicker class="timePicker" reference="menu" v-model="time_today" :config="{collapse:true}" /> </client-only> -->
                  </div>
                </b-button>
              </div>
              <div id="openTimepicker" :class="(isTimeCollapse && timeSwitchCheckedBool) ? 'collapse show' : 'collapse'">
                <div class="date-time-details1">
                  <!-- <vue-timepicker v-model="yourTimeValue" format="HH:mm:ss" /> -->
                </div>
              </div>
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
      selectedTime: '',
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
      isCameraOpen: false, // camera options
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: '#',
      cameraImages: [],
      isDateCollapse: false,
      switchCheckedBool: false,
      isTimeCollapse: false,
      timeSwitchCheckedBool: false,
      yourTimeValue: {
        HH: '10',
        mm: '05',
        ss: '00'
      }
    }
  },
  mounted () {
    this.selDel()
  },
  methods: {
    collapsePicker (pickerID) {
      if (pickerID === 'datepicker') {
        this.isDateCollapse = !this.isDateCollapse
      } else {
        this.isTimeCollapse = !this.isTimeCollapse
      }
    },
    switchChecked (event, picker) {
      if (picker === 'datepicker') {
        if (event.target.checked) {
          this.switchCheckedBool = true
        } else {
          this.switchCheckedBool = false
        }
      } else if (picker === 'timepicker') {
        if (event.target.checked) {
          this.timeSwitchCheckedBool = true
        } else {
          this.timeSwitchCheckedBool = false
        }
      }
    },
    selDel () {
      const dt = new Date(this.date_today)
      this.selectedDate = dt.getDate() + '/' + dt.getMonth() + 1 + '/' + dt.getFullYear()
      this.selectedTime = this.due_time = dt.getHours() + ':' + dt.getMinutes() + ':' + dt.getSeconds()
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
    validateTitle: function (e) {
      const title = e.target.value
      if (title === '') {
        this.enableSubmit = false
        this.isError = true
      } else {
        this.enableSubmit = true
        this.isError = false
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

      for (let d = 0; d < this.cameraImages.length; d++) {
        data.append('cameraImages[' + d + ']', this.cameraImages[d])
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
          // empty form data
          this.title = ''
          this.description = ''
          this.date_today = new Date()
          this.due_time = new Date()
          this.attachment = []
          this.filesdata = []
          // reset all errors
          this.enableSubmit = false
          this.isError = false
          // reload reminders
          this.$emit('reminders')
          alert(response.data.msg)
          // hide popup
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
      this.cameraImages.splice(event, 1)
    },
    openDelete (index) {
      this.slideDelete = !this.slideDelete
      this.fileIndex = index
    }, // camera open
    toggleCamera () {
      if (this.isCameraOpen) {
        this.isCameraOpen = false
        this.isPhotoTaken = false
        this.isShotPhoto = false
        this.stopCameraStream()
      } else {
        this.isCameraOpen = true
        this.createCameraElement()
      }
    },
    stopCameraStream () {
      const tracks = this.$refs.camera.srcObject.getTracks()
      tracks.forEach((track) => {
        track.stop()
      })
    },
    createCameraElement () {
      this.isLoading = true
      const constraints = (window.constraints = {
        audio: false,
        video: true
      })
      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          this.isLoading = false
          this.$refs.camera.srcObject = stream
        })
        .catch((error) => {
          this.isLoading = false
          alert("May the browser didn't support or there is some errors.", error)
        })
    },
    takePhoto () {
      if (!this.isPhotoTaken) {
        this.isShotPhoto = true
        const FLASH_TIMEOUT = 50
        setTimeout(() => {
          this.isShotPhoto = false
        }, FLASH_TIMEOUT)
      }
      this.isPhotoTaken = !this.isPhotoTaken
      this.isLoading = false
      const context = this.$refs.canvas.getContext('2d')
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5)
    },
    downloadImage () {
      document.getElementById('photoTaken').toDataURL('image/jpeg')
        .replace('image/jpeg', 'image/octet-stream')
      const img = document.getElementById('photoTaken').toDataURL('image/png')
      this.filesdata.push(img)
      this.cameraImages.push([img])
      this.takePhoto()
    }
  }
}
</script>

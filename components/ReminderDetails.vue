<template>
  <div class="new-reminderdetails-modal-box">
    <div class="reminder-header">
      <button class="cancle-reminder" @click="$emit('close')">
        <img src="../static/images/left.svg">
        Details
      </button>
    </div>
    <div class="list-slides" :class="{ 'change-slide': changeslide }">
      <div class="list-view">
        <ul>
          <li>
            <h6>{{ data.title }}</h6>
            <p>{{ data.description }}</p>
            <p>
              <img v-for="(file, index) in JSON.parse(data.attachment)" :key="index" :src="url + file" style="height: 130px;width: 25%;margin-left: 10px;">&nbsp;
            </p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ReminderDetails',
  props: {
    data: {
      type: Array,
      default: () => [],
      url: process.env.NUXT_ENV_API_URL
    }
  },
  data () {
    return {
      changeslide: false
    }
  }
}
</script>

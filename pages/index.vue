<template>
  <div class="main-layout">
    <div class="search-box">
      <SearchBox />
    </div>
    <div class="reminder-list">
      <MyList />
    </div>
    <button class="new-reminder" @click="modalactive = !modalactive">
      <img src="../static/images/plus-circle.svg" />
      New Reminder
    </button>
    <div class="new-reminder-modal" :class="{ 'modal-active': modalactive }">
      <ReminderModal />
    </div>
  </div>
</template>

<script>
import '../static/css/style.css'
export default {
  name: 'IndexPage',
  data () {
    return {
      modalactive: false
    }
  }
}
</script>

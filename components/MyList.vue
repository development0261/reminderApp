<!-- Please remove this file from your project -->
<template>
  <div class="my-list-section">
    <h2>My Lists</h2>
    <div class="my-list">
      <ul>
        <li>
          <a href="javascript:void(0)" @click="modalactive = !modalactive">
            <span class="list-icon icon-orange">
              <img src="../static/images/list.svg">
            </span>
            <h6>Reminder</h6>
            <p>{{ count }}</p>
            <img src="../static/images/right.svg">
          </a>
        </li>
      </ul>
    </div>
    <div class="new-list-modal" :class="{ 'modal-active': modalactive }">
      <ListModal @close="modalactive = !modalactive" />
    </div>
  </div>
</template>

<script>
import '../static/css/style.css'
export default {
  name: 'MyList',
  props: {
    count: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      modalactive: false
    }
  }
}
</script>

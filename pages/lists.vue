<template>
  <div class="main-layout">
    <div class="back-to-list">
        <a href="/">
            <img src="../static/images/left.svg" />
            List
        </a>
    </div>
    <h2 class="list-view-title">Reminder</h2>
    <div class="list-view">
        <ul>
            <li>
                <h6>List Title 1</h6>
                <p>List Title 1</p>
            </li>
            <li>
                <h6>List Title 1</h6>
                <p>List Title 1</p>
            </li>
            <li>
                <h6>List Title 1</h6>
                <p>List Title 1</p>
            </li>
            <li>
                <h6>List Title 1</h6>
                <p>List Title 1</p>
            </li>
        </ul>
    </div>
  </div>
</template>

<script>
import '../static/css/style.css'
export default {
  name: 'ListsPage'
}
</script>

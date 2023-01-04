<!-- Please remove this file from your project -->
<template>
    <div class="new-reminder-modal-box">
        <div class="reminder-header" :class="{ 'inner-header': changeslide }">
            <button class="cancle-reminder">Cancel</button>
            <button class="back-reminder" @click="changeslide = !changeslide"><img src="../static/images/left.svg" />New Reminder</button>
            <h4 class="new-reminder-title">New Reminder</h4>
            <h4 class="details-title">Details</h4>
            <button class="add-reminder add-reminder-disable">Add</button>
        </div>
        <div class="reminder-slides" :class="{ 'change-slide': changeslide }">
            <div class="new-reminder-slide">
                <div class="new-reminder-titles">
                    <input type="text" placeholder="Title" />
                    <textarea rows="4" placeholder="Notes" />
                </div>
                <button class="reminder-details-btn" @click="changeslide = !changeslide">
                    Details
                    <img src="../static/images/right.svg" />
                </button>
                <div class="add-image-box">
                    <b-dropdown text="Add Image">
                        <b-dropdown-item-button>
                            <label for="take-photo">
                                <input id="take-photo" type="file" />
                                <p>Take Photo</p>
                                <img src="../static/images/camera.svg" />
                            </label>
                        </b-dropdown-item-button>
                        <b-dropdown-item-button>
                            <label for="take-photo">
                                <input id="take-photo" type="file" />
                                <p>Scan Document</p>
                                <img src="../static/images/scan.svg" />
                            </label>
                        </b-dropdown-item-button>
                        <b-dropdown-item-button>
                            <label for="take-photo">
                                <input id="take-photo" type="file" />
                                <p>Photo Library</p>
                                <img src="../static/images/images.svg" />
                            </label>
                        </b-dropdown-item-button>
                    </b-dropdown>
                    <div class="add-image-list">
                        <ul>
                            <li class="ask-delete">
                                <div class="upload-image-box">
                                    <span>
                                        <img src="../static/images/minus-circle.svg" />
                                    </span>
                                    <img src="../static/images/upload-img.jpg" />
                                    <h6>Image</h6>
                                </div>
                                <button class="delete-upload">Delete</button>
                            </li>
                            <li>
                                <div class="upload-image-box">
                                    <span>
                                        <img src="../static/images/minus-circle.svg" />
                                    </span>
                                    <img src="../static/images/upload-img.jpg" />
                                    <h6>Image</h6>
                                </div>
                                <button class="delete-upload">Delete</button>
                            </li>
                            <li>
                                <div class="upload-image-box">
                                    <span>
                                        <img src="../static/images/minus-circle.svg" />
                                    </span>
                                    <img src="../static/images/upload-img.jpg" />
                                    <h6>Image</h6>
                                </div>
                                <button class="delete-upload">Delete</button>
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
                                    <span class="date-time-switch-slider"></span>
                                </label>
                                <b-button v-b-toggle.collapse-1 class="date-time-btn">
                                    <span class="date-time-icon date-icon">
                                        <img src="../static/images/calendar.svg" />
                                    </span>
                                    <div class="date-time-text">
                                        <h6>Date</h6>
                                        <p>Tuesday, 13 December 2022</p>
                                    </div>
                                </b-button>
                            </div>
                            <b-collapse id="collapse-1">
                                <div class="date-time-details">
                                    <h2>test</h2>
                                </div>
                            </b-collapse>
                        </li>
                        <li>
                            <div class="date-time-btn-outer">
                                <label class="date-time-switch">
                                    <input type="checkbox">
                                    <span class="date-time-switch-slider"></span>
                                </label>
                                <b-button v-b-toggle.collapse-2 class="date-time-btn">
                                   <span class="date-time-icon time-icon">
                                        <img src="../static/images/time.svg" />
                                    </span>
                                    <div class="date-time-text">
                                        <h6>Time</h6>
                                        <p>14:00</p>
                                    </div>
                                </b-button>
                            </div>
                            <b-collapse id="collapse-2">
                                <div class="date-time-details">
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
      changeslide: false
    }
  }
}
</script>

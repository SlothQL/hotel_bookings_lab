<template>
  <div class="detail-wrapper">
      <div class="detail-item">{{ booking.firstName }} {{ booking.surname }}</div>
      <div class="detail-item">|</div>
      <div class="detail-item">{{ booking.email }} </div>
      <div class="detail-item">|</div>
      <div v-if="booking.checkedIn === false" class="detail-item not-checked-in">Not checked in</div>
        <div v-if="booking.checkedIn === true" class="detail-item checked-in">Checked in</div>
        <button v-on:click="handleDelete" class="detail-item btn">Delete Booking</button>
        
        <form v-on:submit="changeCheckedIn" method="put">
        <input type="submit" v-if="booking.checkedIn" class="detail-item check-out" value="Check out" />
        <input type="submit" v-if="!booking.checkedIn" class="detail-item check-in" value="Check in" />
        </form>
  </div>
</template>

<script>
import BookingService from '../services/BookingService.js';
import { eventBus } from '@/main.js';
export default {
    name: 'booking-detail',
    props: ['booking'],
    methods: {
        handleDelete() {
            BookingService.deleteBooking(this.booking._id)
            .then(() => eventBus.$emit('booking-deleted', this.booking._id))
        },

        changeCheckedIn(e) {
            const updated = {
                checkedIn: !this.booking.checkedIn
            }
            BookingService.putBooking(this.booking._id, updated)
                .then( res => eventBus.$emit('updated-booking', res, this.booking._id));
        }
    }
}
</script>

<style scoped>
.detail-wrapper {
    display: flex;
    border: 1px solid darkblue;
    margin-bottom: 15px;
    padding: 10px;
    background-color: white;
    width: 700px;
}

.detail-item {
    margin-left: 10px;
}

.not-checked-in {
    color: red;
}

.checked-in {
    color: green;
}

.check-in {
    background-color: green;
    color: white;
    border: none;
    font-weight: bold;
    padding: 5px;
}

.check-in:hover {
    background-color: mediumseagreen;
    cursor: pointer;
}

.check-out {
    background-color: red;
    color: white;
    border: none;
    font-weight: bold;
    padding: 5px;
}

.check-out:hover {
    background-color: #d15c5c;
    cursor: pointer;
}

</style>
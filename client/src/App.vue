<template>
  <div class="wrapper">
    <h1>Booking Manager</h1>
    <booking-form />
    <h2>All Bookings:</h2>
    <booking-list :bookings="bookings" />
  </div>
</template>

<script>
import { eventBus } from "@/main.js";
import BookingService from './services/BookingService.js';
import BookingForm from './components/BookingForm';
import BookingList from './components/BookingList';
export default {
  name: 'app',
  data() {
    return {
      bookings: []
    }
  },
  mounted() {
    this.fetchBookings();

    eventBus.$on('booking-deleted', (id) => {
      const index = this.bookings.findIndex(booking => booking._id === id);
      this.bookings.splice(index, 1);
    });

    eventBus.$on('new-booking', (booking) => {
      this.bookings.push(booking);
    });

    eventBus.$on('updated-booking', (updated_booking) => {
      console.log(updated_booking);
      const index = this.bookings.findIndex(booking => booking._id === updated_booking._id);
      this.bookings[index].checkedIn = updated_booking.checkedIn;
    })
  },
  components: {
    'booking-form': BookingForm,
    'booking-list': BookingList
  },
  methods: {
    fetchBookings() {
      BookingService.getBookings()
      .then(bookings => this.bookings = bookings);
    }
  }
}
</script>

<style>
body {
  font-family: 'Lato', sans-serif;
  background-color: lightblue;
  margin-left: 50px;
  margin-right: 50px;
}

.btn {
  background-color: darkblue;
  color: white;
  border: none;
  font-weight: bold;
  padding: 5px;
}

.btn:hover {
  background-color: mediumblue;
  cursor: pointer;
}

button {
  outline: none;
}

#submit {
  outline: none;
}

</style>
<template>
  <div>
      <form id="booking-form" v-on:submit.prevent="handleSubmit" method="post">
          <h3>Add a new booking:</h3>
          <div class="form-item">
            <label for="firstName">First Name:</label>
            <input type="text" name="firstName" id="first-name" v-model="firstName" required/>
          </div>
        <div class="form-item">
            <label for="surname">Surname:</label>
            <input type="text" name="surname" id="surname" v-model="surname" required/>
        </div>
        <div class="form-item">
            <label for="email">Email:</label>
            <input type="text" name="email" id="email" v-model="email" required/>
        </div>
        <div class="form-item">
            <input type="submit" value="Submit" id="submit" class="btn"/>
        </div>
      </form>
  </div>
</template>

<script>
import { eventBus } from '@/main.js';
import BookingService from '../services/BookingService.js';
export default {
    name: 'booking-form',
    data() {
        return {
            firstName: '',
            surname: '',
            email: '',
            checkedIn: false
        }
    },
    methods: {
        handleSubmit(e) {
            const booking = {
                firstName: this.firstName,
                surname: this.surname,
                email: this.email,
                checkedIn: this.checkedIn
            }
            BookingService.postBooking(booking)
            .then( res => eventBus.$emit('new-booking', res));
            this.firstName = '';
            this.surname = '';
            this.email = '';
        }
    }
}
</script>

<style scoped>
#booking-form {
    border: 2px solid darkblue;
    padding: 0px 30px 15px 30px;
    margin-bottom: 40px;
    width: 250px;
    background-color: white;
}

.form-item {
    margin-bottom: 10px;
}

label {
    margin-right: 10px;
}

</style>
<template>
  <div id="shop">
    <!-- Cancel project notification -->
    <v-snackbar
      :timeout="3000"
      :bottom="true"
      v-model="cancelpurchase"
    >
      Purchase is cancelled!
      <v-btn flat class="pink--text">Close</v-btn>
    </v-snackbar>
    <v-stepper v-model="purchasestep" elevation-0>
      <v-stepper-header>
        <v-stepper-step step="1" v-bind:complete="purchasestep > 1">Select date and sessions</v-stepper-step>
        <v-divider />
        <v-stepper-step step="2" v-bind:complete="purchasestep > 2">Select tickets</v-stepper-step>
        <v-divider />
        <v-stepper-step step="3">Review and pay</v-stepper-step>
      </v-stepper-header>
      <v-stepper-content step="1">
        <p>Your ticket price includes a visit to the Chapel where you will see an exhibition of the history of the College, the world's largest fan vaulted ceiling and the Adoration of the Magi by Rubens. You will also have access to the College grounds (when open) including the Xu Zhimo stone. Please note all other buildings are private.</p>
        <Calendar></Calendar>
        <v-btn flat @click.native="cancelpurchase = true, purchasestep = 1" class="grey lighten-2"><v-icon left>remove_circle</v-icon>Cancel</v-btn>
        <v-btn color="primary" @click.native="purchasestep = 2" :disabled="!(this.$store.getters['purchase/getdate'] != '' && this.$store.getters['purchase/getsession'] != '')">Continue</v-btn>
      </v-stepper-content>
      <v-stepper-content step="2">
        <Tickets></Tickets>
        <v-btn flat @click.native="cancelpurchase = true, purchasestep = 1" class="grey lighten-3"><v-icon left>remove_circle</v-icon>Cancel</v-btn>
        <v-btn flat @click.native="purchasestep = 1" class="grey lighten-3"><v-icon left>arrow_back</v-icon>Back</v-btn>
        <v-btn color="primary" @click.native="purchasestep = 3" :disabled="!this.$store.getters['purchase/get_ticketvalidation']">Continue</v-btn>
      </v-stepper-content>
      <v-stepper-content step="3">
        <BookingSummary></BookingSummary>
        <v-btn flat @click.native="cancelpurchase = true, purchasestep = 1" class="grey lighten-3" :disabled="(this.$store.getters['purchase/get_paymentstatus'])"><v-icon left>remove_circle</v-icon>Cancel</v-btn>
        <v-btn flat @click.native="purchasestep = 2" class="grey lighten-3" :disabled="(this.$store.getters['purchase/get_paymentstatus'])"><v-icon left>arrow_back</v-icon>Back</v-btn>
      </v-stepper-content>
    </v-stepper>
  </div>
</template>

<script>
import BookingSummary from './components/BookingSummary'
import Calendar from './components/Calendar'
import Tickets from './components/Tickets'

export default {
  name: 'shop',
  components: {
    BookingSummary,
    Calendar,
    Tickets
  },
  data () {
    return {
      cancelpurchase: false,
      purchasestep: 0
    }
  },
  created () {
    const uuidV4 = require('uuid/v4')
    this.$store.dispatch('purchase/setuuid', uuidV4())
    this.purchasestatus = this.$store.getters['purchase/get_paymentstatus']
  }
}
</script>

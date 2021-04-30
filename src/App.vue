<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">Ticket Booking</div>

      <v-spacer></v-spacer>
<!-- hai -->
      <!-- <v-btn
      >
        <span class="mr-2">Upload Excel</span>
      </v-btn> -->
    </v-app-bar>

    <v-main>
      <!-- <GetTicket />
      <Ticket/> -->
      <router-view class="animated fadeIn" />
    </v-main>
  </v-app>
</template>

<script>
// import GetTicket from "./components/GetTicket";
// import Ticket from "./components/Ticket";
export default {
  name: "App",

  components: {
    // GetTicket,
    // Ticket
  },

  data: () => ({
    //
  }),
};
</script>

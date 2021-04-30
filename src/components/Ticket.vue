<template>
  <fragment>
    <div
      id="app"
      class="email-container"
      style="padding: 10px; border: 1px solid lightgray; color: black"
    >
      <div
        class="logo-container"
        style="border-bottom: 1px solid grey; display: flex"
      >
        <img
          src="../assets/bus-logo.png"
          alt=""
          width="100px"
          height="70px"
          style="border-right: 1px solid grey; object-fit: contain"
        />
        <div
          class="logo"
          style="
            font-weight: bolder;
            font-size: 16px;
            color: #e6683c;
            padding: 0px 10px;
          "
        ></div>
        <div
          class="order-id"
          style="
            position: absolute;
            top: 27px;
            right: 24px;
            font-size: 12px;
            text-align: right;
          "
        >
          <span>Email</span> : <span>{{ $route.params.email }}</span>
          <div>Ph: {{ $route.params.ph }}</div>
        </div>
      </div>
      <div style="padding: 10px; font-size: 14px">
        <div>
          Hi <span style="font-weight: 600">{{ $route.params.name }}</span>
        </div>
        <div
          class="confirm"
          style="color: green; font-size: 14px; padding-top: 5px"
        >
          <span style="color: green">&#10004;</span> Your booking is confirmed!!
        </div>
      </div>
      <div style="padding: 10px">
        <table>
          <tr>
            <th style="background: #d21e72; color: white">Ticket</th>
          </tr>
        </table>
      </div>
      <div
        style="
          padding-left: 10px;
          display: flex;
          border-bottom: 1px solid lightgrey;
        "
      >
        <div class="heading" style="width: 40%">
          <h4>Ticket ID</h4>
          <p style="font-size: 12px">{{ ticket.ticketId }}</p>
        </div>
        <div class="heading" style="width: 40%">
          <h4>Seat Number</h4>
          <p style="font-size: 12px">{{ ticket.seatNumber }}</p>
        </div>
        <div class="heading" style="width: 20%">
          <h4>Date</h4>
          <p style="font-size: 12px">{{ currentDate }}</p>
        </div>
      </div>
      <div>
        <div style="padding: 10px">
          NOTE:
          <span style="font-size: 13px"
            >Always carry ticket printout and your ID proof while
            travelling!!</span
          >
        </div>
      </div>
    </div>
    <v-row justify="center" class="pt-10">
      <v-btn color="primary" @click="downloadPdf">
        <span class="mr-2">Download Pdf</span>
      </v-btn>
    </v-row>
  </fragment>
</template>

<script>
import axios from "axios";
import moment from "moment";
import { Fragment } from "vue-fragment";
export default {
  name: "Ticket",
  components: { Fragment },
  data: () => ({
    ticket: "",
    currentDate: "",
  }),
  created() {
    let date = new Date();
    this.currentDate = moment(date).format("L");
  },
  methods: {
    getTicket() {
      axios
        .get("/ticket/findOne/")
        .then((response) => {
          this.ticket = response.data;
          console.log("response", this.ticket);
        })
        .catch((e) => {
          console.log("err", e);
        });
    },
    downloadPdf() {
      const elem = document.getElementById("app");
      window.html2pdf().from(elem).save("ticket");
    },
  },
  mounted() {
    this.getTicket();
  },
};
</script>

<style scoped>
table {
  border-collapse: collapse;
  width: 100%;
  font-size: 14px;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 1.25%;
}
@media screen and (min-width: 446px) {
  .order-id {
    width: 100%;
  }
}
</style>

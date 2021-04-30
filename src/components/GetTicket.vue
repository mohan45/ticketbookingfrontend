<template>
  <v-container>
    <v-dialog v-model="dialog" persistent max-width="350">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="primary" dark v-bind="attrs" v-on="on">
          Upload Excel
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">Import CSV file</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <div class="input-container">
              <input type="file" accept=".xlsx" @change="onImportFileChange" />
            </div>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" text @click="dialog = false">
            Cancel
          </v-btn>
          <v-btn color="green darken-1" text @click="addTicketByImport">
            Ok
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row class="text-center" align="center">
      <v-col cols="12" class="my-2"> GET TICKET </v-col>
      <v-col cols="12"> Enter your details, and Check your tickets </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="4" align-self="center">
        <v-text-field v-model="name" label="Name"></v-text-field>
        <v-text-field v-model="email" label="Email"></v-text-field>
        <v-text-field v-model="phoneNumber" label="Phone Number"></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-btn color="primary" @click="sendUserDetails">
        <span class="mr-2">Get Ticket</span>
      </v-btn>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
import XLSX from "xlsx";
export default {
  name: "GetTicket",

  data: () => ({
    dialog: false,
    name: "",
    email: "",
    phoneNumber: "",
    csvFileName: "",
    seatNumber: "",
    parser: "",
  }),
  methods: {
    async onImportFileChange(e) {
      var file = e.target.files[0];
      var reader = new FileReader();
      reader.onload = (e) => {
        var data = new Uint8Array(e.target.result);
        var workbook = XLSX.read(data, { type: "array" });
        let sheetName = workbook.SheetNames[0];
        let worksheet = workbook.Sheets[sheetName];
        this.parser = XLSX.utils.sheet_to_json(worksheet);
      };
      reader.readAsArrayBuffer(file);
    },
    addTicketByImport() {
      axios
        .post("/tickets/create", {
          data: this.parser,
        })
        .then(() => {
          this.dialog = false;
        })
        .catch((e) => {
          console.log("err", e);
        });
    },
    sendUserDetails() {
      this.$router.push({ name: "Ticket", params: { name: this.name, email: this.email, ph: this.phoneNumber } });
    },
  },
};
</script>

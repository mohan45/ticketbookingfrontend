<template>
  <div class="home">
    <GetTicket msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import GetTicket from "@/components/GetTicket.vue";

export default {
  name: "Home",
  components: {
    GetTicket,
  },
};
</script>

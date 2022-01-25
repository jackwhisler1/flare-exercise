<template>
  <h2>Table</h2>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      flights: [],
    };
  },
  created: function () {
    this.getFlightData();
  },
  methods: {
    getFlightData: function () {
      axios.get("https://flare-code-exercise-data.s3.amazonaws.com/airlines.json").then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>

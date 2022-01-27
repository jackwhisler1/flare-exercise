<template>
  <div class="container-md p-2">
    <h1 class="p-2">Flight Data</h1>
    <!-- Set these terms dynamically when user selects dropdown -->
    <h2>Cancelled Flights in 2008</h2>
    <table v-if="totalFlightData.length" class="table table-hover">
      <thead>
        <tr>
          <th scope="col" v-for="month in months" v-bind:key="month.id">{{ month }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="airport in sortedByAirport" v-bind:key="airport.id">
          <th>{{ airport[0].Airport.Code }}</th>
          <td v-for="month in airport" v-bind:key="month.id">
            {{ calculatePercentage(month.Statistics.Flights.Cancelled, month.Statistics.Flights.Total) }}%
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      totalFlightData: [],
      annualData: [],
      months: [" ", "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
      sortedByAirport: [],
      chunkedArray: [],
    };
  },
  created: function () {
    this.getFlightData();
  },
  mounted: function () {},
  methods: {
    getFlightData: function () {
      axios.get("https://flare-code-exercise-data.s3.amazonaws.com/airlines.json").then((response) => {
        console.log(response.data);
        this.totalFlightData = response.data;
        // Hardcoded for testing
        this.setYear(2008);
        // Hardcoded for testing
        this.setFlightStatusAttribute("Statistics.Flights.Cancelled");
      });
    },
    calculatePercentage: function (number, total) {
      return ((100 * number) / total).toFixed(1);
    },
    setYear: function (year) {
      this.annualData = this.totalFlightData.filter((flight) => flight.Time.Year === year);
      // organize array by airport
      this.sortedByAirport = this.annualData.sort((a, b) => {
        return a.Airport.Code.localeCompare(b.Airport.Code);
      });
      this.sortedByAirport = this.chunkArray(this.sortedByAirport, 12);
      // console.log(this.sortedByAirport);
      // console.log(this.annualData);
      // console.log("setting year");
    },
    setFlightStatusAttribute: function () {
      // placeholder for user dropdown selection
    },
    chunkArray: function (arr, val) {
      for (var i = 0; i < arr.length; i += val) {
        this.chunkedArray.push(arr.slice(i, val + i));
      }
      return this.chunkedArray;
    },
  },
};
</script>

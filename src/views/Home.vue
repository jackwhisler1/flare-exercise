<template>
  <div class="container-md p-5">
    <h2 class="p-2">Flight Data</h2>
    <table v-if="flightData.length" class="table table-striped">
      <thead>
        <tr>
          <th v-for="month in months" v-bind:key="month.id">{{ month }}</th>
        </tr>
      </thead>
      <tbody>
        <tr div v-for="airport in sortedByAirport" v-bind:key="airport.id">
          {{ airport[0].Airport.Code }}
          <td v-for="month in airport" v-bind:key="month.id">
            {{ month.Statistics.Flights.Cancelled }}
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
      flightData: [],
      annualData: [],
      airportCodes: [],
      months: ["", "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
      requestedStatistics: [],
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
        this.flightData = response.data;
        response.data.forEach(() => {
          // placeholer
        });
        // 29 different airports
        for (let i = 0; i < 29; i++) {
          this.airportCodes.push(response.data[i].Airport.Code);
        }
        this.setYear(2008);
        this.setFlightStatusAttribute("Statistics.Flights.Cancelled");
      });
      console.log(this.airportCodes);
    },
    calculatePercent: function (number, total) {
      console.log(Math.ceil(number / total) + "%");
      return Math.ceil(number / total) + "%";
    },
    setYear: function (year) {
      this.annualData = this.flightData.filter((flight) => flight.Time.Year === year);
      // organize array by airport
      this.sortedByAirport = this.annualData.sort((a, b) => {
        return a.Airport.Code.localeCompare(b.Airport.Code);
      });
      this.sortedByAirport = this.chunkArray(this.sortedByAirport, 12);
      console.log(this.sortedByAirport);
      console.log(this.annualData);
      console.log("setting year");
    },
    setFlightStatusAttribute: function () {
      // this.statistics = this.calculatePercent(this.annualData[attribute], this.annualData.Statistics.Total);
      this.requestedStatistics = this.annualData.Statistics.Flights.Cancelled;
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

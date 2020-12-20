<template>
  <h1>
    BootcampMatch: Enroll at the best tech bootcamp for you in 30 minutes or
    less
  </h1>
  <table>
    <tr>
      <th>Name</th>
      <th>Total placement in 90 days for graduated job seekers</th>
      <th>Total placement in 180 days for graduated job seekers</th>

      <th>Offers ISA</th>
      <th>ISA Upfront Fee</th>
    </tr>
    <tr v-for="bootcamp in filteredBootcamps" :key="bootcamp.id">
      <td>
        <a :href="bootcamp.url">{{ bootcamp.name }}</a>
      </td>
      <td>
        <span
          v-if="
            bootcamp.most_recent_public_total_placement_in_90_days_rate_for_graudated_job_seekers
          "
        >
          {{
            bootcamp.most_recent_public_total_placement_in_90_days_rate_for_graudated_job_seekers *
            100
          }}%</span
        >
      </td>
      <td>
        <span
          v-if="
            bootcamp.most_recent_public_total_placement_in_180_days_rate_for_graudated_job_seekers
          "
        >
          {{
            bootcamp.most_recent_public_total_placement_in_180_days_rate_for_graudated_job_seekers *
            100
          }}%</span
        >
      </td>
      <td>
        <span v-if="bootcamp.does_offer_isa">Yes</span><span v-else>No</span>
      </td>
      <td>
        <span v-if="!isNaN(bootcamp.isa_up_front_fee)"
          >${{ bootcamp.isa_up_front_fee.toLocaleString() }}</span
        >
      </td>
    </tr>
  </table>
</template>

<script>
import alasql from "alasql";
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      bootcamps: [],
      filteredBootcamps: [],
    };
  },
  mounted: function () {
    var vm = this;
    axios
      .get("/bootcamps.json")
      .then(function(response) {vm.bootcamps = response.data
      vm.queryTheBootcamps()});
  },
  methods: {
    queryTheBootcamps: function () {
      this.filteredBootcamps = alasql("SELECT * FROM ? ORDER BY name", [
        this.bootcamps,
      ]);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
table {
  border-collapse: collapse;
}
th,
td {
  vertical-align: top;
  padding: 5px;
  text-align: left;
  border: 1px solid #000;
}
</style>

<template>
  <FilterCompo />
  <div>
    <table class="table">
      <thead>
        <tr>
          <th>Problem Name</th>
          <th>Type</th>
          <th>Accepted</th>
          <th>Submission</th>
          <th>Acceptance Rate</th>
          <!-- <th>Contest Name</th> -->
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data" :key="item.problemName">
          <td><a :href="item.problemUrl" target="_blank">{{ item.problemName }}</a></td>
          <td class="text-center">{{ item.type }}</td>
          <td class="text-center">{{ item.accepted }}</td>
          <td class="text-center">{{ item.submissions }}</td>
          <td class="text-center">{{ item.acceptanceRate }}</td>
          <!-- <td class="text-center"><a :href="item.contestUrl" target="_blank">{{ item.contestName }}</a></td> -->
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import FilterCompo from './FilterCompo.vue'

export default {
  data() {
    return {
      data: [],
      sortAcceptedAesc: false,
      sortAcceptedDesc: false,
      orignalData: [],
    };
  },
  components: {
    FilterCompo,
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch('https://us-central1-lcpapi.cloudfunctions.net/api/api/problem');
        const jsonData = await response.json();
        this.data = jsonData;
        this.orignalData = jsonData
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    toogleAccepted() {
      if(this.sortAcceptedDesc) {
        this.data = this.data.sort((a, b) => b.accepted - a.accepted)
        this.sortAcceptedDesc = false
      } else if(this.sortAcceptedAesc) {
        this.data = this.data.sort((a, b) => a.accepted - b.accepted)
        this.sortAcceptedAesc = false
      } else {
        this.data = this.orignalData
      }
    }
  },
};
</script>

<style>
.table {
  margin: auto;
}
.table tbody tr {
  height: 50px; /* Adjust as needed */
}

/* Heading background color */
.table thead th {
  background-color: #f2f2f2; /* Or any other color you prefer */
  height: 60px; /* Adjust as needed */
}

/* Outer border */
.table {
  border: 1px solid #ccc;
  border-collapse: collapse;
}

/* Link styling */
.table a {
  color: #007bff; /* Link color */
  text-decoration: none; /* Remove underlines */
}

.table a:hover {
  text-decoration: underline; /* Underline on hover */
}
.table th,
.table td {
  padding: 8px; /* Adjust as needed */
}
</style>

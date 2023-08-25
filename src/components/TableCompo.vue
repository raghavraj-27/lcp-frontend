<template>
    <table class="table">
      <thead>
        <tr>
          <th>Problem Link</th>
          <th>Problem Name</th>
          <th>Type</th>

          <th @click="sortAccepted = (sortAccepted + 1)%3" :style="{ cursor: 'pointer' }">
            Accepted
            <span v-if="sortAccepted === 1"> &#11015;</span>
            <span v-else-if="sortAccepted === 2"> &#11014;</span>
          </th>

          <th @click="sortSubmission = (sortSubmission + 1)%3" :style="{ cursor: 'pointer' }">
            Submission
            <span v-if="sortSubmission === 1"> &#11015;</span>
            <span v-else-if="sortSubmission === 2"> &#11014;</span>
          </th>

          <th @click="sortAcceptanceRate = (sortAcceptanceRate + 1)%3" :style="{ cursor: 'pointer' }">
            Acceptance Rate
            <span v-if="sortAcceptanceRate === 1"> &#11015;</span>
            <span v-else-if="sortAcceptanceRate === 2"> &#11014;</span>
          </th>

          <th @click="sortContestType = (sortContestType + 1) % 3" :style="{ cursor: 'pointer' }">
            Contest Name
            <span v-if="sortContestType === 1"> &#11015;</span>
            <span v-else-if="sortContestType === 2"> &#11014;</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="data.length === 0">
          <td colspan="6" class="text-center"> {{ info }} </td>
        </tr>
        <tr v-for="item in data" :key="item.problemName">
          <td class="text-center"><v-btn :href="item.problemUrl" target="_blank" color="blue">Open</v-btn></td>
          <td>{{ item.problemName }}</td>
          <td class="text-center">{{ item.type }}</td>
          <td class="text-center">{{ item.accepted }}</td>
          <td class="text-center">{{ item.submissions }}</td>
          <td class="text-center">{{ item.acceptanceRate }}</td>
          <td class="text-center"><a :href="item.contestUrl" target="_blank">{{ item.contestName }}</a></td>
        </tr>
      </tbody>
    </table>
</template>

<script>

export default {
  props: ['formData'],
  data() {
    return {
      info: '',
      data: [],
      orignalData: [],
      sortAccepted: 0,
      sortAcceptanceRate: 0,
      sortContestType: 0,
      sortSubmission: 0,
    };
  },
  watch: {
    formData: {
      deep: true,
      handler: 'fetchData',
    },
    sortAccepted: {
      deep: true,
      handler: 'sortAcceptedFunc'
    },
    sortSubmission: {
      deep: true, 
      handler: 'sortSubmissionFunc'
    },
    sortAcceptanceRate: {
      deep: true,
      handler: 'sortAcceptanceRateFunc'
    },
    sortContestType: {
      deep: true,
      handler: 'sortContestTypeFunc'
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      this.info = "Fetching..."
      this.sortAccepted = 0
      this.sortAcceptanceRate = 0
      this.sortContestType = 0
      this.sortSubmission = 0
      if(Object.keys(this.formData).length === 0) {
        try {
          const response = await fetch('https://us-central1-lcpapi.cloudfunctions.net/api/api/problem');
          const jsonData = await response.json();
          this.data = jsonData;
          this.orignalData = jsonData

          if(this.data.length === 0) this.info = 'No Problem'
        } catch (error) {
          this.info = "Couldn't fetched!"
          console.error('Error fetching data:', error);
        }
      } else {
        const obj = {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.formData),
        }
        try {
          const response = await fetch('https://us-central1-lcpapi.cloudfunctions.net/api/api/problem/filter', obj)
          const jsonData = await response.json();
          this.data = jsonData;
          this.orignalData = jsonData

          if(this.data.length === 0) this.info = 'No Problem'
        } catch (error) {
          this.info = "Couldn't fetched!"
          console.error('Error fetching data:', error);
        }
      }
    },
    sortAcceptedFunc() {
      this.sortAcceptanceRate = 0
      this.sortSubmission = 0
      this.sortContestType = 0
      if(this.sortAccepted === 1) {
        this.data = this.data.slice().sort((a, b) => b.accepted - a.accepted);
      } else if(this.sortAccepted === 2) {
        this.data = this.data.slice().sort((a, b) => a.accepted - b.accepted);
      } else {
        this.data = this.orignalData.slice()
      }
    },
    sortSubmissionFunc() {
      this.sortAccepted = 0
      this.sortContestType = 0
      this.sortAcceptanceRate = 0
      if(this.sortSubmission === 1) {
        this.data = this.data.slice().sort((a, b) => b.submissions - a.submissions);
      } else if(this.sortSubmission === 2) {
        this.data = this.data.slice().sort((a, b) => a.submissions - b.submissions);
      } else {
        this.data = this.orignalData.slice()
      }
    },
    sortAcceptanceRateFunc() {
      this.sortAccepted = 0
      this.sortSubmission = 0
      this.sortContestType = 0
      if(this.sortAcceptanceRate === 1) {
        this.data = this.data.slice().sort((a, b) => {
          const rateA = parseFloat(a.acceptanceRate);
          const rateB = parseFloat(b.acceptanceRate);
          return rateB - rateA;
        });
      } else if(this.sortAcceptanceRate === 2) {
        this.data = this.data.slice().sort((a, b) => {
          const rateA = parseFloat(a.acceptanceRate);
          const rateB = parseFloat(b.acceptanceRate);
          return rateA - rateB;
        });
      } else {
        this.data = this.orignalData.slice()
      }
    },
    sortContestTypeFunc() {
      this.sortAccepted = 0
      this.sortAcceptanceRate = 0
      this.sortSubmission = 0
      if(this.sortContestType === 1) {
        this.data = this.data.slice().sort((a, b) => {
          const a_match = a.contestName.match(/\d+/); // Match one or more digits
          const a_contestNumber = a_match ? parseInt(a_match[0]) : null;

          const b_match = b.contestName.match(/\d+/); // Match one or more digits
          const b_contestNumber = b_match ? parseInt(b_match[0]) : null;

          return b_contestNumber - a_contestNumber;
        });
        // this.data = this.data.slice().sort((a, b) => b.contestName.localeCompare(a.contestName));
      } else if(this.sortContestType === 2) {
        this.data = this.data.slice().sort((a, b) => {
          const a_match = a.contestName.match(/\d+/); // Match one or more digits
          const a_contestNumber = a_match ? parseInt(a_match[0]) : null;

          const b_match = b.contestName.match(/\d+/); // Match one or more digits
          const b_contestNumber = b_match ? parseInt(b_match[0]) : null;

          return a_contestNumber - b_contestNumber;
        });
        // this.data = this.data.slice().sort((a, b) => a.contestName.localeCompare(b.contestName));
      } else {
        this.data = this.orignalData.slice()
      }
    }
  },
};
</script>

<style scoped>
.table {
  margin: auto;
  width: 100%;
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

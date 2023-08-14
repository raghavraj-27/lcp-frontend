<!-- eslint-disable-next-line -->
<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="800px"
    >
      <template v-slot:activator="{ props }">
        <v-btn
          color="primary"
          class="filter-btn"
           v-bind="props"
        >
          FILTER
        </v-btn>
      </template>
  
      <v-card>
        <v-card-title>Filter Problems</v-card-title>
        <v-card-text>
            <v-row>
            <v-col cols="12" sm="6">
              <v-select
                label="Problem Type"
                :items="['All', 'A', 'B', 'C', 'D']"
                variant="solo"
                v-model="type"
              ></v-select>
            </v-col>
            <v-col cols="12" sm="6">
              <v-select
                label="Contest Type"
                :items="['Both', 'Weekly', 'Biweekly']"
                variant="solo"
                v-model="contestType"
              ></v-select>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field label="Accepted From" hide-details="auto" v-model="acceptedFrom"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field label="Accepted To" hide-details="auto" v-model="acceptedTo"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field label="Acceptance Rate From" hide-details="auto" v-model="acceptanceRateFrom"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field label="Acceptance Rate To" hide-details="auto" v-model="acceptanceRateTo"></v-text-field>
            </v-col>
          </v-row>
            
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" block @click="saveData">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
  <TableCompo :formData="computedFormData" />
</template>

<script>
import TableCompo from './TableCompo.vue'
  export default {
    components: {
      TableCompo,
    },
    data () {
      return {
        dialog: false,
        type: '',
        acceptedFrom: '',
        acceptedTo: '',
        acceptanceRateFrom: '',
        acceptanceRateTo: '',
        contestType: '',
        sendData: false,
        formData: {},
      }
    },
    computed: {
      computedFormData() {
        return this.formData
      }
    },
    methods: {
      saveData() {
        // const formdata = {}
        if(this.type !== '' && this.type !== null && this.type !== undefined && this.type !== 'All') 
          this.formData.type = this.type
        if(this.contestType !== '' && this.contestType !== null && this.contestType !== undefined && this.contestType !== 'Both') 
          this.formData.contestType = this.contestType
        if(this.acceptedFrom !== '' && this.acceptedFrom !== null && this.acceptedFrom !== undefined) 
          this.formData.acceptedFrom = parseInt(this.acceptedFrom)
        if(this.acceptedTo !== '' && this.acceptedTo !== null && this.acceptedTo !== undefined) 
          this.formData.acceptedTo = parseInt(this.acceptedTo)
        if(this.acceptanceRateFrom !== "" && this.acceptanceRateFrom !== null && this.acceptanceRateFrom !== undefined) 
          this.formData.acceptanceRateFrom = this.acceptanceRateFrom
        if(this.acceptanceRateTo !== '' && this.acceptanceRateTo !== null && this.acceptanceRateTo !== undefined) 
          this.formData.acceptanceRateTo = this.acceptanceRateTo

        // this.formData = formdata
        this.sendData = true
        this.dialog = false
      },
    }
  }
</script>

<style scoped>
.filter-btn {
    margin-bottom: 10px;
}
.v-text-field, .v-select {
  margin: 10px;
}
</style>
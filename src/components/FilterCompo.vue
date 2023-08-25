<!-- eslint-disable-next-line -->
<template>
  

  <v-row class="parent">
    
    <v-col cols="12" md="12" sm="12" lg="12">
        <v-row>
          <v-col cols="12" md="6" sm="6" lg="6" class="text-left">
            <!-- <v-text-field label="Search Problem" hide-details="auto" v-model="acceptedFrom" :width="50"></v-text-field>  -->
          </v-col>
          
          <v-col cols="12" md="6" sm="6" lg="6" class="text-right">
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
                      :items="['A', 'B', 'C', 'D']"
                      variant="solo"
                      v-model="type"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-select
                      label="Contest Type"
                      :items="['Weekly', 'Biweekly']"
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
                <v-row>
                  <v-col cols="4">
                    <v-btn color="blue" block @click="saveData">Submit</v-btn>
                  </v-col>
                  <v-col cols="4">
                    <v-btn color="black" block @click="resetFilters">Reset</v-btn>
                  </v-col>
                  <v-col cols="4">
                    <v-btn color="red" block @click="dialog = false">Close</v-btn>
                  </v-col>
                </v-row>
              </v-card-actions>
            </v-card>
            </v-dialog>
          </v-col>
        </v-row>
    </v-col>
    <v-col cols="12" md="12" sm="12" lg="12">
      <TableCompo :formData="computedFormData" />
    </v-col>

  </v-row>
  
  
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
        if(this.type !== '' && this.type !== null && this.type !== undefined) 
          this.formData.type = this.type
        if(this.contestType !== '' && this.contestType !== null && this.contestType !== undefined) 
          this.formData.contestType = this.contestType
        if(this.acceptedFrom !== '' && this.acceptedFrom !== null && this.acceptedFrom !== undefined) 
          this.formData.acceptedFrom = parseInt(this.acceptedFrom)
        if(this.acceptedTo !== '' && this.acceptedTo !== null && this.acceptedTo !== undefined) 
          this.formData.acceptedTo = parseInt(this.acceptedTo)
        if(this.acceptanceRateFrom !== "" && this.acceptanceRateFrom !== null && this.acceptanceRateFrom !== undefined) 
          this.formData.acceptanceRateFrom = this.acceptanceRateFrom
        if(this.acceptanceRateTo !== '' && this.acceptanceRateTo !== null && this.acceptanceRateTo !== undefined) 
          this.formData.acceptanceRateTo = this.acceptanceRateTo

        this.dialog = false
      },
      resetFilters() {
        this.type = ''
        this.contestType = ''
        this.acceptanceRateFrom = ''
        this.acceptedFrom = ''
        this.acceptedTo = ''
        this.acceptanceRateTo = ''

        this.formData = {}
      }
    }
  }
</script>

<style scoped>
.filter-btn {
    margin-bottom: 10px;
}
.v-text-field, .v-select {
  margin: 2px;
}
.parent {
  width: 90%;
  margin: auto;
}
</style>
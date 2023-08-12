<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="800px"
    >
      <template v-slot:activator="{ props }">
        <v-btn
          color="primary"
          v-bind="props"
          class="filter-btn"
        >
          FILTER
        </v-btn>
      </template>

      <v-card>
        <v-card-text>
            <v-text-field label="Type" hide-details="auto"></v-text-field>
            <v-text-field label="Accepted From" hide-details="auto"></v-text-field>
            <v-text-field label="Accepted To" hide-details="auto"></v-text-field>
            <v-text-field label="Acceptance Rate From" hide-details="auto"></v-text-field>
            <v-text-field label="Acceptance Rate To" hide-details="auto"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" block @click="dialog = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        dialog: false,
      }
    },
  }
</script>

<style scoped>
.filter-btn {
    margin-bottom: 10px;
}
</style>
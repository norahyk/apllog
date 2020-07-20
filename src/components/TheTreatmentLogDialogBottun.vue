<template>
  <v-dialog v-model="dialog" max-width="500px">
    <template #activator="{ on, attrs }">
      <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on"
        >New Item</v-btn
      >
    </template>
    <TheTreatmentLogForm formTitle="New Item" @cancel="cancel" @save="save" />
  </v-dialog>
</template>

<script>
import { db } from "../plugins/firebase";
import TheTreatmentLogForm from "./TheTreatmentLogForm.vue";

export default {
  name: "TheTreatmentLogDialogBottun",
  components: {
    TheTreatmentLogForm,
  },
  data: () => {
    return {
      dialog: false,
    };
  },
  methods: {
    cancel: function() {
      this.dialog = false;
    },
    save: function(e) {
      db.collection("treatments").add(e);
    },
  },
};
</script>

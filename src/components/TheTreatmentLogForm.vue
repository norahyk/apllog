<template>
  <v-card>
    <v-card-title>
      <span class="headline">{{ formTitle }}</span>
    </v-card-title>

    <v-card-text>
      <v-container>
        <v-row>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              v-model.number="treatment.step"
              label="ステップ"
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <FormDatePicker dateTitle="開始日" @picked="setStart" />
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <FormDatePicker dateTitle="終了日" @picked="setEnd" />
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>

    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn color="blue darken-1" text @click="cancel">Cancel</v-btn>
      <v-btn color="blue darken-1" text @click="save">Save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import FormDatePicker from "./FormDatePicker.vue";

export default {
  name: "TheTreatmentLogForm",
  data: () => {
    return {
      treatment: {
        step: 0,
        started: 0,
        ended: 0,
      },
    };
  },
  props: {
    formTitle: {
      type: String,
      require: true,
    },
  },
  methods: {
    cancel: function() {
      this.$emit("cancel");
    },
    save: function() {
      this.$emit("save", this.treatment);
    },
    setStart(e) {
      this.treatment.started = e;
    },
    setEnd(e) {
      this.treatment.ended = e;
    },
  },
  components: {
    FormDatePicker,
  },
};
</script>

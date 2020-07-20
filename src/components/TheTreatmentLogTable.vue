<template>
  <v-data-table
    :headers="headers"
    :items="items"
    :items-per-page="20"
    sort-by="step"
    hide-default-footer
    class="elevation-1"
  >
    <template #top>
      <v-toolbar flat color="white">
        <v-toolbar-title>治療履歴</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <TheTreatmentLogDialogBottun />
      </v-toolbar>
    </template>
    <template #item.started="{ item }">
      {{ formatDate(item.started) }}
    </template>
    <template #item.ended="{ item }">
      {{ formatDate(item.ended) }}
    </template>
    <template #item.actions="{ item }">
      <v-icon small @click="deleteItem(item)">
        mdi-delete
      </v-icon>
    </template>
  </v-data-table>
</template>

<script>
import { db } from "../plugins/firebase";
import TheTreatmentLogDialogBottun from "./TheTreatmentLogDialogBottun.vue";

export default {
  name: "TheTreatmentLogTable",
  data: () => {
    return {
      items: [],
      headers: [
        { text: "治療ステップ", value: "step" },
        { text: "開始日", value: "started" },
        { text: "終了日", value: "ended" },
        { text: "アクション", value: "actions", sortable: false },
      ],
    };
  },
  methods: {
    formatDate(date) {
      if (date) {
        date = date
          .toDate()
          .toISOString()
          .split("T")[0];
      }
      return date;
    },
    deleteItem(item) {
      db.collection("treatments")
        .doc(item.id)
        .delete();
    },
  },
  firestore() {
    return {
      items: db.collection("treatments"),
    };
  },
  components: {
    TheTreatmentLogDialogBottun,
  },
};
</script>

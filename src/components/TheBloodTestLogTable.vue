<template>
  <v-app>
    <v-data-table
      :headers="headers"
      :items="items"
      :items-per-page="200"
      sort-by="tested"
      sort-desc
      hide-default-footer
      class="elevation-1"
    >
      <template #top>
        <v-toolbar flat color="white">
          <v-toolbar-title>血液検査履歴</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
        </v-toolbar>
      </template>
      <template #body.prepend="{ headers }">
        <tr>
          <td><FormDataPicker dateTitle="" @picked="setTested" /></td>
          <td v-for="n in headers.length - 2" :key="n">
            <v-text-field
              v-model.number="editTest[headers[n].value]"
            ></v-text-field>
          </td>
          <td>
            <v-icon small @click="newItem">
              mdi-plus
            </v-icon>
          </td>
        </tr>
      </template>
      <template #item.tested="{ item }">
        {{ formatDate(item.tested) }}
      </template>
      <template #item.actions="{ item }">
        <v-icon small @click="deleteItem(item)">
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>
  </v-app>
</template>

<script>
import { db } from "../plugins/firebase";

import FormDataPicker from "./FormDatePicker.vue";

export default {
  name: "TheBloodTestLogTable",
  data: () => {
    return {
      items: [],
      headers: [
        { text: "採血日", value: "tested" },
        { text: "白血球", value: "wbc" },
        { text: "好中球%", value: "seg_band" },
        { text: "好中球", value: "neutrophil" },
        { text: "赤血球", value: "rbc" },
        { text: "ヘモグロビン", value: "hb" },
        { text: "網状赤血球", value: "retic" },
        { text: "血小板", value: "plt" },
        { text: "CRP", value: "crp" },
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
    newItem() {
      db.collection("blood_test").add(this.editTest);
    },
    deleteItem(item) {
      db.collection("blood_test")
        .doc(item.id)
        .delete();
    },
    setTested(e) {
      this.editTest.tested = e;
    },
  },
  firestore() {
    return {
      items: db.collection("blood_test"),
    };
  },
  computed: {
    editTest() {
      var editTest = {};
      this.headers.map((e) => (editTest[e.value] = ""));
      delete editTest.actions;
      return editTest;
    },
  },
  components: { FormDataPicker },
};
</script>

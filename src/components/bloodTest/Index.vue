<template>
  <v-data-table
    fixed-header
    height="700px"
    :headers="headers"
    :items="tests"
    :items-per-page="200"
    sort-by="tested"
    sort-desc
    class="elevation-1"
  >
    <template v-slot:body.prepend="{ headers }">
      <tr>
        <td>
          <!-- <v-text-field v-model="editedTest.tested" dense hide-details :label="tested" outlined></v-text-field> -->
          <v-menu
            v-model="menu"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field v-model="editedTest.tested" label="tested" v-bind="attrs" v-on="on"></v-text-field>
            </template>
            <v-date-picker v-model="editedTest.tested" @input="menu = false"></v-date-picker>
          </v-menu>
        </td>
        <td v-for="index in headers.length-2" :key="index">
          <v-text-field
            v-model.number="editedTest[headers[index].value]"
            dense
            hide-details
            :label="headers[index].value"
            outlined
          ></v-text-field>
          <!-- {{ editedTest[headers[index-1].value] }} -->
        </td>
        <td>
          <v-btn class="mr-2" fab x-small dark color="warning" @click="add">
            <v-icon>mdi-plus</v-icon>
          </v-btn>
        </td>
      </tr>
    </template>
    <template v-slot:item.actions>
      <v-icon small class="mr-2">mdi-cached</v-icon>
    </template>
  </v-data-table>
</template>

<script>
import firebase from "firebase";
// import InputForm from "./InputForm.vue";

export default {
  name: "testIndex",
  // components: { InputForm },
  data: function() {
    return {
      db: null,
      headers: [
        { text: "検査日", value: "tested" },
        { text: "白血球数", value: "wbc" },
        { text: "好中球％", value: "seg_band" },
        { text: "赤血球", value: "rbc" },
        { text: "ヘモグロビン", value: "hb" },
        { text: "網状赤血球", value: "retic" },
        { text: "血小板", value: "plt" },
        { text: "CRP", value: "crp" },
        { text: "Actions", value: "actions", sortable: false }
      ],
      editedTest: {
        tested: new Date().toISOString().split("T")[0],
        wbc: "",
        seg_band: "",
        rbc: "",
        hb: "",
        retic: "",
        plt: "",
        crp: ""
      },
      tests: [],
      menu: false
    };
  },
  created: function() {
    this.db = firebase.firestore();

    var _this = this;

    this.db
      .collection("blood_test")
      // .orderBy("tested")
      .onSnapshot(function(qs) {
        _this.tests = [];
        qs.forEach(function(doc) {
          var data = doc.data();
          data.id = doc.id;
          data.tested = data.tested
            .toDate()
            .toISOString()
            .split("T")[0];
          _this.tests.push(data);
        });
      });
  },
  methods: {
    add: function() {
      var editedTest = Object.create(this.editedTest);
      console.log(this.editedTest.tested);
      editedTest.tested = new Date(editedTest.tested);
      console.log(this.editedTest.tested);
      this.db.collection("blood_test").add(editedTest);
    }
  }
};
</script>

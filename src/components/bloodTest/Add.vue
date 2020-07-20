<template>
  <div>
    <h1>New Test Result</h1>
    <div>
      <ul>
        <li><input type="date" v-model="tested" /></li>
        <li><input type="number" v-model="wbc" /></li>
        <li><button @click="addTest">Register</button></li>
      </ul>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";
import "firebase/firestore";

export default {
  name: "testAdd",
  data: function() {
    return {
      db: null,
      tested: new Date().getDate,
      wbc: 0,
    };
  },
  created: function() {
    this.db = firebase.firestore();
  },
  methods: {
    addTest: function() {
      this.db.collection("blood_test").add({
        tested: this.tested,
        wbc: this.wbc,
      });
    },
  },
};
</script>

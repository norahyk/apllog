<template>
  <div id="app">
    <label>名前</label>
    <input type="text" v-model="userName" />

    <label>メッセージ</label>
    <input type="text" v-model="message" />
    <button @click="sendMessage">送信</button>
    <ul>
      <li v-for="(value, key, index) in messageList" v-bind:key="index">
        {{ value.user_name }}
        <span style="margin-left:100px;">{{ value.message }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import firebase from "firebase";
let messageRef;

export default {
  name: "app",
  data() {
    return {
      messageList: [],
      userName: "ユーザーA",
      message: "テストメッセージです",
    };
  },
  created: function() {
    // Your web app's Firebase configuration
    var firebaseConfig = {
      apiKey: "AIzaSyDVcyZ2-Fw7kXVE1N2weqtc7l_ePK48JwI",
      authDomain: "facilitater-c58c5.firebaseapp.com",
      databaseURL: "https://facilitater-c58c5.firebaseio.com",
      projectId: "facilitater-c58c5",
      storageBucket: "facilitater-c58c5.appspot.com",
      messagingSenderId: "906277573957",
      appId: "1:906277573957:web:817d6edcec4a64b8965a2c",
      measurementId: "G-2LWS6Y6V2L",
    };
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    firebase.analytics();

    const db = firebase.firestore();
    messageRef = db.collection("chat_messages");

    let messageList = this.messageList;
    messageRef.orderBy("created", "desc").onSnapshot(function(qs) {
      messageList.length = 0;
      qs.forEach((result) => {
        messageList.push(result.data());
      });
    });
  },
  methods: {
    sendMessage: function() {
      messageRef.add({
        user_name: this.userName,
        message: this.message,
        created: new Date().getTime(),
      });
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
  <div>
    <form @submit.prevent="submit">
      <input type="text" v-model="name" />
      <input type="number" v-model="age" />
      <input type="text" v-model="country" />
      <button>Send</button>
    </form>
  </div>
  <ul v-if="load == true">
    <li v-for="info in info" :key="info.id">
      {{ info.name }} is {{ info.age }} from {{ info.country }}
    </li>
  </ul>
  <h1 v-else>Loading</h1>
</template>

<script>
import firebase from "firebase";

var firebaseConfig = {
  apiKey: "AIzaSyAnQtIa6aAMG_PNvt4duwa71qBunYRfccY",
  authDomain: "learning-firebase-basics-72c99.firebaseapp.com",
  projectId: "learning-firebase-basics-72c99",
  storageBucket: "learning-firebase-basics-72c99.appspot.com",
  messagingSenderId: "183375292459",
  appId: "1:183375292459:web:e70ba3c82ee473db0fe26e",
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);

const db = firebase.firestore();
var docRef = db.collection("message").orderBy("age", "desc");
export default {
  data() {
    return {
      name: null,
      age: null,
      country: null,
      info: [],
      load: true,
    };
  },
  methods: {
    submit() {
      if (this.name && this.age && this.country) {
        this.load = false;
        db.collection("message")
        
          .add({
            name: this.name,
            age: this.age,
            country: this.country,
          })
          .then((docRef) => {
            console.log("Document written with ID: ", docRef.id);
            this.load = true;
            this.info = [];
            this.readData();
          })
          .catch(function(error) {
            console.error("Error adding document: ", error);
          });
      }
    },
    readData() {
      docRef.get().then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          // doc.data() is never undefined for query doc snapshots
          this.info.push(doc.data());
        });
      });
    },
  },
  created() {
    this.readData();
  },
};
</script>

<style lang="scss" scoped></style>

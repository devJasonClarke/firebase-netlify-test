<template>
  <div>
    <form @submit.prevent="submit">
      <input type="text" v-model="message" />
      <button>Send</button>
    </form>
  </div>
  <ul>
    <li v-for="info in info" :key="info.id">{{info.message}} {{info.id}}</li>
  </ul>
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
var docRef = db.collection("message");
export default {
  data() {
    return {
      message: null,
      info: []
    };
  },
  methods: {
    submit() {
      if (this.message) {
        db.collection("message")
          .add({
            first: "jason",
            last: "clarke",
            message: this.message,
            born: 2002,
          })
          .then(function(docRef) {
            console.log("Document written with ID: ", docRef.id);
        
          })
          .catch(function(error) {
            console.error("Error adding document: ", error);
          });


      }
      this.info = [];
          this.readData();
    },
    readData(){
        docRef.get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
           
         this.info.push(doc.data());
          });
        });
    }
  },
  created(){
      this.readData()
  }

};
</script>

<style lang="scss" scoped></style>

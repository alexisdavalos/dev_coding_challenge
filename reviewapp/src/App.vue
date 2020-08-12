<template>
  <div id="app">
    <Navbar id="nav" />
    <router-view v-on:submit-review="submit" :reviewItems="reviewItems" />
    <Footer />
    <b-loading :is-full-page="true" :active.sync="isLoading" :can-cancel="true">
      <b-icon pack="fas" icon="sync-alt" size="is-large" custom-class="fa-spin">
      </b-icon>
    </b-loading>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Footer from "@/components/Footer.vue";
import moment from "moment";
// Firebase Import
import Firebase from "firebase/app";
import "firebase/database";
import "firebase/storage";

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: `${process.env.VUE_APP_FIREBASE_API_KEY}`,
  authDomain: `${process.env.VUE_APP_FIREBASE_AUTH_DOMAIN}`,
  databaseURL: `${process.env.VUE_APP_FIREBASE_DATABASE_URL}`,
  projectId: `${process.env.VUE_APP_FIREBASE_PROJECT_ID}`,
  storageBucket: `${process.env.VUE_APP_STORAGE_BUCKET}`,
  messagingSenderId: `${process.env.VUE_APP_FIREBASE_SENDER_ID}`,
  appId: `${process.env.VUE_APP_FIREBASE_APP_ID}`,
};

// Initialize Firebase
Firebase.initializeApp(firebaseConfig);
let db = Firebase.database();
let reviewsRef = db.ref("reviews");

export default {
  name: "reviewApp",
  data() {
    return {
      isLoading: true,
      reviewItems: [],
    };
  },
  created() {
    this.isLoading = true;
  },
  mounted() {
    // Gets Review Data
    this.getData();
  },
  watch: {
    // whenever reviewItems changes, this function will run
    reviewItems: function() {
      this.isLoading = false;
    },
  },
  components: {
    Navbar,
    Footer,
  },
  methods: {
    submit: function(formData) {
      // Sends formData to Firebase Realtime Database
      reviewsRef.push({ ...formData, timestamp: moment().format() });
      // Gets New Review Data
      this.getData();
    },
    getData: function() {
      // Reset Reviews Array
      this.reviewItems.length = 0;
      // Retrieves reviews from Firebase
      reviewsRef.on("value", (reviews) => {
        reviews.forEach((review) => {
          this.reviewItems.push({
            deviceVariant: review.child("deviceVariant").val(),
            message: review.child("message").val(),
            name: review.child("name").val(),
            rating: review.child("rating").val(),
            timestamp: review.child("timestamp").val(),
          });
        });
      });
    },
  },
};
console.log;
</script>

<style lang="scss">
@import "./styles/_colors.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav a {
  font-weight: bold;
}

#nav a.router-link-exact-active {
  color: $primary;
}
</style>

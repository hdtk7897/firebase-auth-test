<template>
  <div>
    uid:{{ user.uid }}
  </div>
</template>

<script>
import firebase from "firebase";

export default {
  data() {
    return {
      user: {},
      authLock: false,
    };
  },

  mounted: function() {
    console.log('mount')
    this.initFirebase();
  },

  methods: {
    initFirebase: function() {
      if (!firebase.apps.length) {
        const result = firebase.initializeApp({
          apiKey: process.env.VUE_APP_API_KEY,
          authDomain: process.env.VUE_APP_AUTH_DOMAIN,
          databaseURL: process.env.VUE_APP_DATABASE_URL,
          projectId: process.env.VUE_APP_PROJECT_ID,
          storageBucket: process.env.VUE_APP_STORAGE_BUCKET,
          messagingSenderId: process.env.VUE_APP_MESSAGING_SENDER_ID
        });
        console.log(result)
      }
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          this.user = user
          console.log("authed");
        } else {
          if (this.authLock) {
            console.log("auth cancel");
            return;
          }
          this.authLock = true;
          console.log("authing");
          firebase.auth().signInAnonymously()
        }
      });
    },
  }
};
</script>


<template>
  <div>
    <div v-if="loggedIn">
      <p>Logged in</p>
      <button @click="signOut">Sign out</button>
    </div>
    <p v-else>Not logged in</p>
  </div>
</template>

<script>
import * as firebase from "firebase/app";
import "firebase/auth";
export default {
  mounted() {
    this.setupFirebase();
  },
  methods: {
    setupFirebase() {
      firebase.auth().onAuthStateChanged(user => {
        if (user) {
          // User is signed in.
          this.loggedIn = true;
        } else {
          // No user is signed in.
          this.loggedIn = false;
        }
      });
    },
    signOut() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.replace({ name: "login" });
        });
    }
  },
  data() {
    return {
      loggedIn: false
    };
  }
};
</script>

<style scoped>
p {
  margin: 0;
  padding: 0;
}
</style>
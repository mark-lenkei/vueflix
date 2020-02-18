<template>
  <div>
    <form @submit.prevent="pressed">
      <h1>Register</h1>
      <div>
        <input type="email" v-model="email" placeholder="email" />
      </div>
      <div>
        <input type="password" v-model="password" placeholder="password" />
      </div>
      <button type="submit">Register</button>
    </form>
    <div v-if="error">{{error.message}}</div>
  </div>
</template>

<script>
import * as firebase from "firebase/app";
import "firebase/auth";

export default {
  data() {
    return {
      email: "",
      password: "",
      error: ""
    };
  },
  methods: {
    pressed() {
      firebase
        .auth()
        .createUserWithEmailAndPassword(this.email, this.password)
        .then(() => {
          this.$router.replace({ name: "home" });
        })
        .catch(error => (this.error = error));
    }
  }
};
</script>


<style scoped>
input {
  width: 300px;
  font-size: 18pt;
  margin: 8px;
}

button {
  width: 200px;
  height: 40px;
  color: #2c3e50;
  font-size: 14pt;
  font-weight: bolder;
  background-color: #42b983;
  margin-bottom: 5px;
}
</style>
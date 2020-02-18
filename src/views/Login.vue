<template>
  <div>
    <form @submit.prevent="pressed">
      <h1>Login</h1>
      <div>
        <input type="email" v-model="email" placeholder="email" />
      </div>
      <div>
        <input type="password" v-model="password" placeholder="password" />
      </div>
      <button type="submit">Login</button>
    </form>
    <div v-if="error">{{error.message}}</div>
    <p>
      Click here to
      <router-link to="/register" class="register">register</router-link>
    </p>
    <p>
      If you forgot you password
      <router-link to="/resetpassword" class="register">click here</router-link>
    </p>
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
        .signInWithEmailAndPassword(this.email, this.password)
        .then(
          /*data*/ () => {
            this.$router.replace({ name: "home" });
          }
        )
        .catch(error => {
          this.error = error;
        });
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

p {
  font-size: 14pt;
}
.register {
  color: #2c664c;
}
</style>
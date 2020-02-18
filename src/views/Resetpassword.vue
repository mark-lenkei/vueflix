<template>
  <div>
      <form @submit.prevent="pressed">
      <h1>Reset password</h1>
      <div>
        <input type="email" v-model="email" placeholder="email">
      </div>
      <button type="submit">Reset</button>
    </form>
    <div v-if="message">{{message}}</div>
  </div>
</template>

<script>
import * as firebase from "firebase/app";
import "firebase/auth";

  export default {
      data() {
          return{
              email: "",
              message: ""
          }
      },
      methods: {
          pressed(){
              firebase.auth().sendPasswordResetEmail(this.email)
              .then(()=>{
                this.message="The password reset email has been sent to "+this.email;
              })
              .catch((err)=>{
                  this.message=err.message;
              })
          }
      }
  }
</script>

<style scoped>
input{
  width: 300px;
  font-size:18pt;
  margin: 8px;
}

button{
  width: 200px;
  height: 40px;
  color: #2c3e50;
  font-size: 14pt;
  font-weight: bolder;
  background-color: #42b983;
  margin-bottom: 5px;
}

p{
  font-size:14pt;
}
</style>
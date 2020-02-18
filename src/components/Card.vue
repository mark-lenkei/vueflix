<template>
  <div v-on:click="increment" class="container" v-bind:style="{ 'background-image': 'url(' + serie.image + ')' }">
      <div class="layer">
        <h2 class="name">{{serie.name}}</h2>
        <h4 class="type">{{serie.type}}</h4>
      </div>
  </div>
</template>

<script>
import firebase from 'firebase';
import { db } from '../main';

  export default {
      props: ["serie"],
      methods: {
        increment(){
          let increment = firebase.firestore.FieldValue.increment(1);
          let seenRef = db.collection('series').doc(this.serie.id);
          seenRef.update({reads: increment});
          let type = this.serie.type;
          let currentUser = firebase.auth().currentUser.uid;
          let userDocRef = db.collection('users').doc(currentUser);
          return db.runTransaction(function(transaction){
            return transaction.get(userDocRef).then(function(userDoc){
              if(type==='drama'){
                let seen = userDoc.data().drama+1;
                transaction.update(userDocRef, {drama: seen});
              } else if(type==='comedy'){
                let seen = userDoc.data().comedy+1;
                transaction.update(userDocRef, {comedy: seen});
              } else if(type==='family'){
                let seen = userDoc.data().family+1;
                transaction.update(userDocRef, {family: seen});
              } else if(type==='fantasy'){
                let seen = userDoc.data().fantasy+1;
                transaction.update(userDocRef, {fantasy: seen});
              }
            });
          });
        }
      }
  }
</script>

<style scoped>
.container{
    position: relative;
    width:250px;
    height: 141px;
    margin-bottom: 25px;
}

.layer{
    position: absolute;
    top:0;
    left:0;
    width: 100%;
    height: 100%;
    background-color: rgba(66, 185, 131, 0.6);
}

.name{
    position: absolute;
    top: 0px;
    left: 8px;
    color: #2c3e50;
}

.type{
    position: absolute;
    bottom: 0px;
    left: 8px;
    color: #2c3e50;
}
</style>
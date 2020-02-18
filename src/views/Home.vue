<template>
  <div>
    <router-link to="/admin" v-if="isAdmin">
      <h1>Admin Profile</h1>
    </router-link>
    <div v-if="recommended.length>0">
      <h1>Recommended for you</h1>
      <div class="grid-container">
        <div class="max" v-for="serie in recommended" v-bind:key="serie.id">
          <Card v-bind:serie="serie"></Card>
        </div>
      </div>
    </div>
    <h1>Available on VueFlix</h1>
    <div class="grid-container">
      <div class="max" v-for="serie in series" v-bind:key="serie.id">
        <Card v-bind:serie="serie"></Card>
      </div>
    </div>
  </div>
</template>

<script>
import { db } from "../main";
import Card from "../components/Card";
import firebase from "firebase";

export default {
  name: "home",
  components: {
    Card
  },
  data: () => ({
    series: [],
    recommended: [],
    isAdmin: false
  }),
  mounted() {
    this.getCollection("series");
    this.createUser();
    this.getRecommendation();
    this.getAdmins();
  },
  methods: {
    async getCollection(collectionName) {
      let snapshot = await db.collection(collectionName).get();
      let series = [];
      snapshot.forEach(serie => {
        let appData = serie.data();
        appData.id = serie.id;
        series.push(appData);
      });
      this.series = series;
    },
    createUser() {
      let currentUser = firebase.auth().currentUser;
      let userRef = db.collection("users").doc(currentUser.uid);
      userRef.get().then(user => {
        if (!user.exists) {
          db.collection("users")
            .doc(currentUser.uid)
            .set({
              fantasy: 0,
              drama: 0,
              comedy: 0,
              family: 0
            });
        }
      });
    },
    async getRecommendation() {
      let currentUser = firebase.auth().currentUser;
      let snapshot = await db
        .collection("users")
        .doc(currentUser.uid)
        .get();
      snapshot = snapshot.data();
      let recommendedseries = [];
      let recommendedtype = ["comedy", "drama", "family", "fantasy"];
      let typevalue = [
        snapshot.comedy,
        snapshot.drama,
        snapshot.family,
        snapshot.fantasy
      ];
      if (
        snapshot.comedy > 0 ||
        snapshot.drama > 0 ||
        snapshot.family > 0 ||
        snapshot.fantasy > 0
      ) {
        let max = typevalue[0];
        let maxtype = recommendedtype[0];
        for (let i = 1; i < typevalue.length; i++) {
          if (typevalue[i] > max) {
            max = typevalue[i];
            maxtype = recommendedtype[i];
          }
        }
        this.series.forEach(serie => {
          if (serie.type === maxtype) {
            recommendedseries.push(serie);
          }
        });
        this.recommended = recommendedseries;
      }
    },
    async getAdmins() {
      let snapshot = await db.collection("admins").get();
      let currentUser = firebase.auth().currentUser;
      snapshot.forEach(admin => {
        if (admin.id === currentUser.uid) {
          this.isAdmin = true;
        }
      });
    }
  }
};
</script>

<style scoped>
.max {
  max-width: 250px;
  max-height: 141px;
}
.grid-container {
  display: grid;
  grid-template-columns: repeat(5, auto);
  grid-row-gap: 15px;
}
a {
  color: #42b983;
}
</style>

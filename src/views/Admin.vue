<template>
  <div>
    <form @submit.prevent="pressed">
      <h1>Add series</h1>
      <div class="row">
        <label>Name:</label>
        <input type="text" v-model="name" placeholder="Name of the series" />
      </div>
      <div class="row">
        <label>Image link:</label>
        <input type="text" v-model="image" placeholder="Image link" />
      </div>
      <div class="row">
        <label>Number of seasons:</label>
        <input type="number" v-model="seasons" placeholder="How many seasons the series has?" />
      </div>
      <div class="row">
        <label>Number of episodes in one season:</label>
        <input
          type="number"
          v-model="episodesperseason"
          placeholder="How many episodes the series has in one season?"
        />
      </div>
      <label>Type:</label>
      <select v-model="type">
        <option>fantasy</option>
        <option>comedy</option>
        <option>drama</option>
        <option>family</option>
      </select>
      <br />
      <button type="submit">Add series</button>
    </form>
    <h2 v-if="status">Series successfully added</h2>
  </div>
</template>

<script>
import { db } from "../main";

export default {
  data() {
    return {
      name: "",
      image: "",
      seasons: 0,
      episodesperseason: 0,
      type: "",
      status: ""
    };
  },
  methods: {
    pressed() {
      db.collection("series")
        .add({
          name: this.name,
          image: this.image,
          seasons: this.seasons,
          episodesperseason: this.episodesperseason,
          type: this.type
        })
        .then(ref => {
          this.status = ref.id;
        })
        .catch(/*err*/ () => {});
      setTimeout(() => {
        this.status = "";
      }, 5000);
    }
  }
};
</script>

<style scoped>
input {
  width: 300px;
  font-size: 18pt;
  margin: 8px;
  display: table-cell;
}

button {
  width: 200px;
  height: 40px;
  color: #2c3e50;
  font-size: 14pt;
  font-weight: bolder;
  background-color: #42b983;
  margin-bottom: 5px;
  margin-top: 10px;
  display: table-cell;
}
label {
  font-size: 18pt;
  font-weight: bold;
  display: table-cell;
}
select {
  font-size: 18pt;
  display: table-cell;
}
form {
  display: table;
  margin: 0 auto;
}
.row {
  display: table-row;
}
</style>
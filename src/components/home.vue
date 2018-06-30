<template>
<div>
    <header class="nav">
      <span>World Cup Scores</span>
    </header>
    <main>
      <h2>Click button to get scores</h2>
      <button v-on:click="getScores">Get Scores</button>
      <div  v-if="loading">
          <img src="../assets/logo.png" alt="Vue logo">
          Loading
      </div>
      <div v-for="matchday of rounds" class="matchday-div">
        <div class="matchday">
          <div class="matchday-title">
            <h4>{{matchday.name}}</h4>
          </div>
          <div v-for="match of matchday.matches" class="matchday-body">
            <ul>
              <li>{{match.city  }}</li>
            </ul>
          </div>
        </div>
      </div>
    </main>
    </div>
</template>
<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      scores: [],
      loading: false
    };
  },
  methods: {
    getScores: function() {
      this.loading = true;
      axios
        .get(
          "https://raw.githubusercontent.com/openfootball/world-cup.json/master/2018/worldcup.json"
        )
        .then(
          response => {
            this.loading = false;
            this.scores = response.data.rounds;
            console.log(response + " ROUNDS -> " + this.scores[0]);
          },
          error => {
            this.loading = false;
          }
        );
    }
  }
};
</script>
<style lang="scss">
.matchday {
  display: flex;
  & > ul {
    list-style: none;
  }
}
</style>

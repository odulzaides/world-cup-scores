<template>
  <div id="app">
    <header class="nav">
      <h1>World Cup Scores</h1>
    </header>
    <main>
      <div>
        <h4>Select Matchday</h4>
        <select v-model="matchday"
                v-on:change="getMatchdayMatches(matchday)" >
          <option  
                    v-for="matchday of rounds" 
                    :value="matchday.name">{{matchday.name}}</option>
        </select>
        <div class="row">
          <div v-if="match.score1 >= 0"
          v-for="match of matches"
               class="col-md-4 match">  
            <div>
              <div class="title-div">
                <h3>{{match.group}}</h3>
                <h5 v-if="match.group">{{match.city}}</h5>
                <h3 v-if="match.num <57"  >Round of 16</h3>
                <h3 v-if="match.num <57"  >Quarter Finals</h3>

                <h5>{{match.date}}</h5>
              </div>
              <div class="team-div">
                <p>{{match.team1.name}}</p>
                <p>{{match.score1}}</p>
              </div>
              <div class="team-div">
                <p>{{match.team2.name}}</p>
                <p>{{match.score2}}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      matchday: "",
      matches: "",
      rounds: [],
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
            this.rounds = response.data.rounds;
          },
          error => {
            this.loading = false;
          }
        );
    },
    getMatchdayMatches: function() {
      localStorage.matchday = this.matchday;
      for (var i = 0; i < this.rounds.length; i++) {
        if (this.rounds[i].name == this.matchday) {
          this.matches = this.rounds[i].matches;
          localStorage.matches = this.rounds[i];
          console.log(this.matches);
        }
      }
    }
  },
  beforeMount() {
    this.getScores();
    // this.getMatchdayMatches();
    if (localStorage.matchday) {
      this.matchday = localStorage.matchday;
      // this.matches = JSON.plocalStorage.matches;
    }
  }
};
</script>
<style lang="scss">
#app {
  margin: 0;
  padding: 0;
}
.body {
  margin: 0;
  padding: 0;
}
.nav {
  background: rgba(0%, 52.2%, 79.2%, 1);
  color: white;
  height: 3em;
}
.nav > h1 {
  margin: 0 auto;
  padding: 0.2em 0.8em;
}
.row {
  display: flex;
  flex-wrap: wrap;
}
.col-md-4 {
  width: 25%;
}
.match {
  background: transparent;
  color: black;
  flex-grow: 1;
  padding: 1em;
  margin: 10px 0 0 2%;
  border-radius: 10px;
  border: 1px lightgrey solid;
  flex: 1 0 25%;
}
.title-div {
  margin: 0 auto;
}
.team-div {
  display: flex;
  padding-right: 2em;
  margin: 1em;
}
.team-div > p:first-child {
  margin-right: 2em;
}
</style>

<template>
  <div id="app">
    <AsteroidGrid :asteroid="asteroid" header="Near Earth " @remove="remove"/>
  </div>
</template>

<script>
  import AsteroidGrid from './components/AsteroidGrid.vue'
  import axios from 'axios'

export default {
  name: 'App',
  components: {
    AsteroidGrid
  },
  data() {
    return{
        asteroid: [],
      }
      },
      created: function () {
        this.fetchAsteroid();
      },
      methods: {
        testPhrase2() {
          return "this is fun";
        },
        process: function (event) {
          event.preventDefault();
          this.submitted = 2;
          alert("looks like you made it user " + this.email);
        },
        process2: function (event) {
          event.preventDefault();
          this.submitted = 3;
        },
        unprocess: function (event) {
          event.preventDefault();
          this.submitted = 1;
        },

        fetchAsteroid: function () {
          var apiKey = "IGBvvSGLWhba8cxyG5Xa3oWUxjx5xaYNXoYRPiWG";
          var URL = "https://api.nasa.gov/neo/rest/v1/neo/browse?api_key=" + apiKey;
          axios.get(URL).then(response2=> {
            // handle success
            this.asteroid = response2.data.near_earth_objects.slice(0, 10);
          });
        },

        remove: function (index) {
          this.asteroid.splice(index, 1);
        }
      }
    }
  </script>

<style>
  [v-cloak]{
                display: none;
            }
</style>

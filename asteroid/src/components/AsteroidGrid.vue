<template>
  <div class="card mt-5">
    <h2>{{header}}<transition name="spin" appear><Span style="display:inline-block">Object</Span></transition>
    </h2>
    <transition name="shooting-star">
      <div class="mt-3" v-cloak v-show="showDataSummary">
        <span v-cloak v-if="numAsteroid>1">showing {{numAsteroid}} items</span>
        <span v-cloak v-else-if="numAsteroid===1">showing {{numAsteroid}} item</span>
        <span v-cloak>
          <p>{{closestObject}} has the shortest miss distance from Earth</p>
        </span>
      </div>

    </transition>
    <div class="mt-3">
      <a href="#" @click="showDataSummary=!showDataSummary">Show/Hide Data Summary</a></div>
    <table class="table table-striped">
      <thead class="thead-dark">
        <th>#</th>
        <th>Name</th>
        <th>Close Approach Date</th>
        <th>Miss Approach Date</th>
        <th>Remove</th>
      </thead>
      <tbody is="transition-group" name="rise-up" v-cloak>
        <tr v-for="(a, index) in asteroid" :key="newtr(index)" :class="{highlight: isMisingData(a)}">
          <td>{{index+1}}</td>
          <td>{{a.name}}</td>
          <td>{{getCloseApproachDate(a)}}</td>
          <td>
            <ul v-if="a.close_approach_data.length>0">
              <li v-for="(value, key) in a.close_approach_data[0].miss_distance" v-bind:key="key">
                {{value}}:{{key}}
              </li>
            </ul>
          </td>
          <td>
            <button @click="remove(index)" class="btn btn-warning">
              remove
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        showDataSummary: true
      }
    },
    computed: {
      numAsteroid: function () {
        return this.asteroid.length;
      },
      closestObject: function () {
        //filter, map and sort fucntion
        var filter = this.asteroid.filter(function (a) {
          return a.close_approach_data.length > 0;
        });
        var map = filter.map(function (a) {
          return { name: a.name, miles: a.close_approach_data[0].miss_distance.miles }
        });
        var sort = map.sort(function (a, b) {
          return a.miles - b.miles;
        })
        if (sort.length > 0) {
          return sort[0].name;
        }
        else { return null; }
      }
    },
    methods: {
      getCloseApproachDate: function (a) {
        if (a.close_approach_data.length > 0) {
          return a.close_approach_data[0].close_approach_date;
        }
        else {
          return "N/A";
        }
      },
      remove: function (index) {
        this.$emit('remove', index);
      },
      getRowStyle: function (a) {
        if (a.close_approach_data.length === 0) {
          return { border: ' solid red', color: 'red' };
        }
      },
      isMisingData: function (a) {
        if (a.close_approach_data.length === 0) {
          return true;
        }
      },
      newtr: function (index) {
        return "tr" + index;
      }
    },
  }
</script>
<style>

  .highlight {
    border: solid red;
    color: red;
  }

  .shooting-star-enter-active,
  .shooting-star-leave-active {
    transition: all .5s ease;
  }

  .shooting-star-leave-to,
  .shooting-star-enter {
    transform: translateX(150px) rotate(30deg);
    opacity: 0;
  }

  .rise-up-leave-to,
  .rise-up-enter {
    transform: translateY(30px);
    opacity: 0;
  }

  .rise-up-enter-active,
  .rise-up-leave-active {
    transition: all 1s linear;
  }

  .spin-enter-active {
    /*problem with this is it will play upon enter the page since all of the div got rendered*/
    /*bump up the time or make the div not rendered directly and go in consequence to prevent this*/
    animation: spin-steps 6s;
  }

  @keyframes spin-steps {
    0% {
      transform: scale(1) rotate(0);
    }

    50% {
      transform: scale(5) rotate(360deg);
    }

    100% {
      transform: scale(1) rotate(1080deg);
    }
  };
</style>
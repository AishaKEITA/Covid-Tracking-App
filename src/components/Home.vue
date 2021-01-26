<template>
 <v-app id="home">
   <v-main>
<h1>The current US values - Total deaths</h1>
<v-container fluid>
        <v-row align="center">
        <v-col
          class="d-flex align-center"
          cols="12"
          sm="6"
        >
        <v-select backgroundColor="#DDD" color="blue" @change="changeState" v-model="stateSelectedByUser"
          :hint="`Deaths: ${deathsBySingleState} - Date: ${dateBySingleState} `"
            :items="states"
            label="Choose a state"
          ></v-select>
          <span>Selected: {{ stateSelectedByUser }}</span>
        </v-col>
      </v-row>
        </v-container>
      <div class
      ="container">
     <div class ="chart">
       <Bar
        v-if="loaded"
        :chartdata="chartdata"/>
      </div>
    </div>
      <div v-for="(info, index) in covidData" :key="index">
        <div v-for="(value, key) in info" :key="key">
         <!-- key: {{key}}: value: {{value}} -->
        </div>
      </div>
      </v-main>
  </v-app>
</template>

<script>
import Bar from './Chart'
export default {

  components: {Bar},
  // eslint-disable-next-line no-dupe-keys
  name: 'Home',
  data () {
    // death for a specific state for the last 30 days
    // update the chart when getting the last 30 days
    return {
      loaded: false,
      chartdata: null,
      covidData: [],
      states: [],
      deaths: [],
      dates: [],
      deathsBySingleState: '',
      dateBySingleState: '',
      stateSelectedByUser: '',
      deathsByStates: '',
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },

  // simple fetch of the api
  created () {
    this.loaded = false
    fetch('https://api.covidtracking.com/v1/states/current.json')
      .then(response => response.json())
      .then(data => {
        this.covidData = data
        // console.log(this.covidData)
        for (let covidInfo of this.covidData) {
          // console.log(covidInfo.death)
          // this.death = covidInfo.death
          this.deaths.push(covidInfo.death)
          this.dates.push(covidInfo.date)
          this.states.push(covidInfo.state)
          // console.log(covidInfo.date)
          // console.log(covidInfo.state)
        }

        // this.chartdata = death
        this.chartdata = {
          labels: this.states,
          datasets: [{
            label: 'Death',
            backgroundColor: '#f87979',
            data: this.deaths,
            borderWidth: 1,
            barPercentage: 0.5,
            barThickness: 6,
            minBarLength: 1,
            maxBarThickness: 8
          }
          ]

        }
        this.loaded = true
      })
  },
  methods: {
    /* fetch daily deaths in all states */
    async getDeathsByStates (state) {
      try {
        const response = await fetch(`https://api.covidtracking.com/v1/states/${state}/daily.json`)
        const data = await response.json()

        if (!response.ok) {
          throw new Error(`Got error code: ${response.status} Error: ${data.error}`)
        }
        this.deathsByStates = data
        // console.log(this.deathsByStates[0].date)
        console.log(this.deathsByStates[0].death)
        this.deathsBySingleState = this.deathsByStates[0].death
        this.dateBySingleState = this.deathsByStates[0].date
      } catch (err) {
        console.log(err)
      }
    },
    /* calling the getDeathByStates function through changeState event */
    changeState (event) {
      const state = event.toLowerCase()
      console.log(state)
      this.getDeathsByStates(state)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: gray;
  text-align: center;

}

</style>

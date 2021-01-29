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
            :items="allStatesNames"
            label="Choose a state and see death for the last 30 days"
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
        :chartData="chartdata"/>
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
    // update the chart when getting the last 30 day
    return {
      loaded: false,
      chartdata: null,
      covidData: [],
      allStatesNames: [],
      allStatesDeaths: [],
      last30daysDeaths: [],
      last30daysDates: [],
      deathsBySingleState: '',
      dateBySingleState: '',
      stateSelectedByUser: '',
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },

  /* --- simple fetch of the api --- */
  created () {
    this.loaded = false
    fetch('https://api.covidtracking.com/v1/states/current.json')
      .then(response => response.json())
      .then(data => {
        this.covidData = data
        for (let covidInfo of this.covidData) {
          this.allStatesDeaths.push(covidInfo.death)
          this.allStatesNames.push(covidInfo.state)
        }

        this.chartdata = {
          labels: this.allStatesNames,
          datasets: [{
            label: 'Death',
            backgroundColor: '#f87979',
            data: this.allStatesDeaths,
            borderWidth: 1,
            barPercentage: 0.5,
            barThickness: 6,
            minBarLength: 1,
            maxBarThickness: 8,
            type: 'bar'
          }
          ]
        }
        this.loaded = true
      })
  },
  methods: {
    /* --- fetch daily deaths in a states --- */
    async getDeathByState (state) {
      try {
        const response = await fetch(`https://api.covidtracking.com/v1/states/${state}/daily.json`)
        const data = await response.json()

        if (!response.ok) {
          throw new Error(`Got error code: ${response.status} Error: ${data.error}`)
        }

        /* --- loop to get specific deaths for 30 days --- */
        let deaths = []
        let dates = []
        let last30days = data.slice(0, 30)
        for (let dailyData of last30days) {
          deaths.push(dailyData.death)
          dates.push(dailyData.date)
        }

        deaths.reverse()
        dates.reverse()

        this.last30daysDeaths = deaths
        this.last30daysDates = dates

        this.deathsBySingleState = data[0].death
        this.dateBySingleState = data[0].date

        this.chartdata = {
          labels: this.last30daysDates,
          datasets: [{
            label: 'Deaths last 30 days',
            backgroundColor: 'blue',
            data: this.last30daysDeaths,
            borderWidth: 1,
            barPercentage: 0.5,
            barThickness: 6,
            minBarLength: 1,
            maxBarThickness: 8,
            type: 'line',
            fill: false
          }
          ]
        }
      } catch (err) {
        console.log(err)
      }
    },
    /* --- calling the getDeathByState function through changeState event --- */
    changeState (event) {
      const state = event.toLowerCase()
      this.getDeathByState(state)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: gray;
  text-align: center;
  font-size: 40px;
  font-family:monospace;
}
span {
  margin-left: 10px;
  font-size: 25px;
  color: blue;
}

</style>

<template>
  <div id="home">
  <v-app id="inspire">
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
        <th class="text-left">Date</th>
          <th class="text-left">Date Checked</th>
            <th class="text-left"> Death</th>
            <th class="text-left">Death Increase</th>
            <th class="text-left">Hash</th>
            <th class="text-left">Hospitalized</th>
            <th class="text-left">Hospitalized Cumulative</th>
            <th class="text-left">Hospitalized Currently:</th>
            <th class="text-left">hospitalized Increase</th>
            <th class="text-left">Inicummulative</th>
            <th class="text-left">Inicucurrently</th>
            <th class="text-left">Last Modified</th>
            <th class="text-left">Negative</th>
            <th class="text-left">Negative Increase</th>
            <th class="text-left">Onventilator Cumulative </th>
            <th class="text-left">OnVentilator Currently</th>
            <th class="text-left">Pending</th>
            <th class="text-left">PosNeg</th>
            <th class="text-left">Positive</th>
            <th class="text-left">Positive Increase</th>
            <th class="text-left">Recovered</th>
            <th class="text-left">States</th>
            <th class="text-left">Total</th>
            <th class="text-left">Total Test Results</th>
            <th class="text-left">Total Test Results Increase</th>
          </tr>
        </thead>
        <tbody>
          <tr
           v-for=" posts in post" :key="posts.date"
          >
            <td>{{posts.date}} </td>
            <td>{{posts.dateChecked}} </td>
            <td>{{posts.death}} </td>
            <td>{{posts.deathIncrease}} </td>
            <td>{{posts.hash}} </td>
            <td>{{posts.hospitalized}} </td>
            <td>{{posts.hospitalizedCumulative}} </td>
             <td>{{posts.hospitalizedCurrently}} </td>
            <td>{{posts.hospitalizedIncrease}} </td>
            <td>{{posts.inIcuCumulative}} </td>
            <td>{{posts.inIcuCurrently}} </td>
            <td>{{posts.lastModified}} </td>
            <td>{{posts.negative}} </td>
            <td>{{posts.negativeIncrease}} </td>
            <td>{{posts.onVentilatorCumulative}} </td>
            <td>{{posts.onVentilatorCurrently}} </td>
            <td>{{posts.pending}} </td>
            <td>{{posts.posNeg}} </td>
            <td>{{posts.positive}} </td>
            <td>{{posts.positiveIncrease}} </td>
            <td>{{posts.recovered}} </td>
            <td>{{posts.states}} </td>
            <td>{{posts.total}} </td>
            <td>{{posts.totalTestResults}} </td>
            <td>{{posts.totalTestResultsIncrease}} </td>
            </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-app>
</div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      post: null
    }
  },
  // simple fetch of the api
  created () {
    fetch('https://api.covidtracking.com/v1/us/current.json')
      .then(response => response.json())
      .then(data => {
        this.post = data
        console.log(this.post)
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

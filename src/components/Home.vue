<template>
  <div id="home">
  <v-app id="inspire">
      <div>
  <ul>
      <li v-for="(value, key) in post" :key="key">
         {{ key }} : {{ value }}
     </li>
    </ul>
  </div>
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
//   methods () {

//   }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

<template>
  <div id="home">
  <v-app id="inspire">
    <div>
    </div>
  </v-app>
</div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {

    }
  },
  // simple fetch of the api
  created () {
    fetch('https://api.covidtracking.com/v1/us/current.json')
      .then(response => response.json())
      .then(data => {
        console.log(data)
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

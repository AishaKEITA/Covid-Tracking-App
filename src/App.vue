<template>
  <v-app>
    <v-app-bar app color="#228" dark border="white">
      <div
        style="cursor: pointer"
        @click="$router.push('/').catch(err => {})"
        class="d-flex align-center"
      >
        <v-icon class="mr-3" size="40">mdi-pound</v-icon>
        <span style="font-size: 18px"><img class="logo" src="./assets/covid-tracker-logo.jpeg" alt=""></span>
      </div>

      <v-spacer></v-spacer>
      <v-btn @click="$router.push('/').catch(() => {})" text>
        <span>Home</span>
        <!-- <v-icon>mdi-open-in-new</v-icon> -->
      </v-btn>
      <v-btn class="ml-2" @click="$router.push('/About').catch(() => {})" text>
        <span>About Us</span>
        <!-- <v-icon>mdi-open-in-new</v-icon> -->
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>

</template>

<script>
export default {
  name: 'app',

  components: {

  },

  data: () => ({
    //
  })
}
</script>
<style>
html {
  overflow-y: hidden !important;
}
* {
  box-sizing: border-box !important;
}
.logo {
  width: 40%;
}
</style>

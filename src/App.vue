<template>
  <v-app id="app-container">
    <v-app-bar app dark>
      <v-toolbar-title class="headline text-uppercase">
        <span>Techtonic</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-content>
    <Home />
    </v-content>
  </v-app>
</template>

<script>

import Home from './views/Home.vue';
export default {
  name: 'App',
  components: {
    Home
  },
  data: () => ({
    
  }),
};
</script>

<style>
#app-container {
  background-color: rgb(241, 241, 241);
}
</style>

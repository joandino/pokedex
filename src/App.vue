<template>
  <v-app>
    <v-app-bar
      color="red"
      dark
    >
      <v-toolbar-title>Pokemon</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <Home />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import Home from './components/Home.vue';

export default Vue.extend({
  name: 'App',

  components: {
    Home,
  },

  data: () => ({
    //
  }),
});
</script>

<style lang="scss">
#app {
  background-image: url('~@/assets/pokeball.jpg');
  background-repeat: repeat;
  background-attachment: fixed;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100vh;
}
</style>

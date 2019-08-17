<template>
  <v-app>
    <Navbar/>
    <v-content  class="grey darken-2">
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from './components/Navbar.vue';

export default {
  components:{
    Navbar
  },
  data: () => ({
    //
  }),
};
</script>

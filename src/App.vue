<template>
  <v-app class="app-wallpaper" style="background-image: url('/img/wallpapers/nightsong.jpg');background-attachment: fixed;background-position: center;background-repeat: no-repeat;background-size: cover;">
    <app-navbar />

    <v-content>
      <v-container>
        <app-carousel />
        <app-table id="Table" />
      </v-container>
    </v-content>

    <app-footer class="m-100" />
  </v-app>
</template>

<script>
import Navbar from './components/Navbar'
import Footer from './components/Footer'
import Table from './components/Table'
import Carousel from './components/Carousel'

export default {
  name: 'app',
  components: {
    'app-navbar': Navbar,
    'app-footer': Footer,
    'app-table': Table,
    'app-carousel': Carousel,
  },
}
</script>

<style>
html {
  scroll-behavior: smooth;
}

.m-100 {
  margin-top: 15rem;
}
</style>
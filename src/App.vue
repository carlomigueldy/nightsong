<template>
  <v-app>
    <app-navbar />

    <v-content>
      <v-container>
        <app-carousel />
        <app-table />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from './components/Navbar'
import Table from './components/Table'
import Carousel from './components/Carousel'

export default {
  name: 'app',
  components: {
    'app-navbar': Navbar,
    'app-table': Table,
    'app-carousel': Carousel,
  },
}
</script>
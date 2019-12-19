<template>
  <v-app>
    <app-navbar />

    <v-content>
      <v-container>
        <app-table />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from './components/Navbar'
import Table from './components/Table'

export default {
  name: 'app',
  components: {
    'app-navbar': Navbar,
    'app-table': Table,
  },
}
</script>
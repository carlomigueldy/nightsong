<template>
    <v-footer
    dark
    padless
    >
        <v-card
        flat
        tile
        width="100%"
        class="white--text text-center"
        >
        <v-card-text class="white--text">
          We are a group of friends playing together for fun.
        </v-card-text>

        <v-divider></v-divider>

        <v-card-text class="white--text">
            {{ new Date().getFullYear() }} — <strong>The Nightsong Guild</strong>
        </v-card-text>
        </v-card>
    </v-footer>
</template>

<script>
  export default {
    data: () => ({
      icons: [
        'fab fa-facebook',
        'fab fa-twitter',
        'fab fa-google-plus',
        'fab fa-linkedin',
        'fab fa-instagram',
      ],
    }),
  }
</script>
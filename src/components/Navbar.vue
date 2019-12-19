<template>
    <v-app-bar
        dark
        app
        color="indigo darken-4"
        >
        <v-toolbar-title>
            <a href="#" class="headline" style="text-decoration: none; color: white;">Nightsong Armory</a>
        </v-toolbar-title>
        <v-spacer></v-spacer>

        <v-btn 
            :disabled="music" 
            @click="playBackgroundMusic()" 
            text 
            dark>
            Play Music
        </v-btn>
        <v-btn href="#Table" text dark>Who's Online?</v-btn>

        <v-menu
            left
            bottom
        >
            <template v-slot:activator="{ on }">
            <v-btn icon v-on="on">
                <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
            </template>

            <v-list>
            <v-list-item>
                <v-list-item-title>Built by <a href="https://carlomigueldy.com" target="_blank">carlomigueldy</a></v-list-item-title>
            </v-list-item>
            </v-list>
        </v-menu>
    </v-app-bar>
</template>

<script>
export default {
    data: () => ({
        music: false,
    }),
    methods: {
        playBackgroundMusic() {
            this.music = true
            const audio = new Audio('/music/song.mp3');
            audio.play();
        },
    },
}
</script>
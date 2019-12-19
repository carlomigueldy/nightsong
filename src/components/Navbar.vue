<template>
    <v-app-bar
        app
        clipped-left
        >
        <v-toolbar-title>Nightsong Armory</v-toolbar-title>
        <v-spacer></v-spacer>

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
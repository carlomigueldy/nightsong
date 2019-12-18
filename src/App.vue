<template>
  <v-app>
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

    <v-content>
      <v-container>
        <v-card :loading="loading">
          <v-card-title>
            Guild Members
            <v-spacer></v-spacer>
            <!-- <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field> -->
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="members"
            :search="search"
          >
            <template v-slot:item.class="{ item }">
              <img :src="classAvatar(item.class)" height="35">
            </template>
            <template v-slot:item.online="{ item }">
              <v-chip :color="item.online ? 'green lighten-1' : 'red lighten-1'">{{ item.online ? 'Online' : 'Offline' }}</v-chip>
            </template>
            <template v-slot:item.professions="{ item }">
              <ul>
                <li v-for="(profession, index) in item.professions.professions" :key="index">
                  {{ profession.name }} ({{ profession.skill }})
                </li>
              </ul>
            </template>
          </v-data-table>
        </v-card>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data: () => ({
    loading: false,
    search: '',
    leader: {},
    members: [],
    headers: [
      {
        text: 'Class',
        align: 'left',
        sortable: true,
        value: 'class',
      },
      {
        text: 'Status',
        align: 'left',
        sortable: true,
        value: 'online',
      },
      {
        text: 'Level',
        align: 'left',
        sortable: true,
        value: 'level',
      },
      {
        text: 'Name',
        align: 'left',
        sortable: true,
        value: 'name',
      },
      {
        text: 'Gender',
        align: 'left',
        sortable: true,
        value: 'gender',
      },
      {
        text: 'Race',
        align: 'left',
        sortable: true,
        value: 'race',
      },
      {
        text: 'Achievement Points',
        align: 'left',
        sortable: true,
        value: 'achievementpoints',
      },
      {
        text: 'Professions',
        align: 'left',
        sortable: false,
        value: 'professions',
      },
    ]
  }),
  created() {
    this.fetchNightsong()
  },
  methods: {
    /**
     * Fetches the data from Warmane API.
     * 
     * @return { void }
     */
    async fetchNightsong() {
      this.loading = true
      try {
        const res = await axios.get('https://cors-anywhere.herokuapp.com/https://armory.warmane.com/api/guild/Nightsong/Lordaeron/summary')
        this.leader = res.data.leader
        this.members = res.data.roster
        this.loading = false
        console.log(res.data)
      } catch (err) {
        this.loading = false
        console.log(err)
      }    
    },

    /**
     * Returns the link of a corresponding class.
     * 
     * @param { String } class
     */
    classAvatar(class_name) {
      switch (class_name) {
        case 'Death Knight':
          return '/img/death_knight.png'
        case 'Druid':
          return '/img/druid.png'
        case 'Hunter':
          return '/img/hunter.png'
        case 'Mage':
          return '/img/mage.png'
        case 'Paladin':
          return '/img/paladin.png'
        case 'Priest':
          return '/img/priest.png'
        case 'Rogue':
          return '/img/rogue.png'
        case 'Shaman':
          return '/img/shaman.png'
        case 'Warlock':
          return '/img/warlock.png'
        case 'Warrior':
          return '/img/warrior.png'
      }
    },

    /**
     * Fetches a single resource, a charcter info.
     * 
     * @param { String } name
     */
    fetchCharacter(name) {
      console.log(name)
    },
  },
}
</script>
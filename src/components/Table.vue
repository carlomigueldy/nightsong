<template>
  <div>
    <v-card :loading="loading">
      <v-card-title>
        Guild Members of the Nightsong
        <v-spacer></v-spacer>
        <!-- <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field> -->
        <v-btn 
          @click="refreshData()"
          :disabled="disable"
          :loading="refreshing" 
          color="primary">
          <v-icon>mdi-refresh</v-icon>
          Refresh
        </v-btn>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="members"
        :search="search"
        item-key="id"
      >
        <template v-slot:item.class="{ item }">
          <img :src="classAvatar(item.class)" height="35">
        </template>
        <template v-slot:item.online="{ item }">
          <v-chip :color="item.online ? 'green lighten-1' : 'red lighten-1'">
            {{ item.online ? 'Online' : 'Offline' }}
          </v-chip>
        </template>
        <template v-slot:item.professions="{ item }">
          <ul>
            <li v-for="(profession, index) in item.professions.professions" :key="index">
              {{ profession.name }} ({{ profession.skill }})
            </li>
          </ul>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-btn 
            depressed 
            :loading="item.loading"
            :disabled="item.disabled"
            color="grey darken-2" 
            @click="fetchCharacter(item.name, item.index)">
            <v-icon>mdi-eye</v-icon>
          </v-btn>
        </template>
      </v-data-table>
    </v-card>

    <v-dialog
        v-model="characterInfo"
        width="500"
      >
      <v-card>
        <v-card-title
          primary-title
        >
          {{ character.name }}
        </v-card-title>

        <v-card-text>
          <div>Status: {{ character.online }}</div>
          <div>Name: {{ character.name }}</div>
          <div>Level: {{ character.level }}</div>
          <div>Gender: {{ character.gender }}</div>
          <div>Race: {{ character.race }}</div>
          <div>Class: {{ character.class }}</div>
          <div>Honorable Kills: {{ character.honorablekills }}</div>
          <div>Achievement Points: {{ character.achievementpoints }}</div>
          <div>Talents: {{ character.talents }}</div>
          <div>Professions: {{ character.professions }}</div>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="characterInfo = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created() {
    this.fetchNightsong()
  },

  data: () => ({
    cors: 'https://cors-anywhere.herokuapp.com',
    api: 'https://armory.warmane.com/api',
    characterInfo: false,
    loading: false,
    disable: false,
    refreshing: false,
    search: '',
    leader: {},
    character: {},
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
      {
        text: 'Actions',
        align: 'center',
        sortable: false,
        value: 'actions',
      },
    ]
  }),

  methods: {
    /**
     * Refreshes the data fetched from the API.
     * 
     * @return { Void }
     */
    refreshData() {
      this.refreshing = true
      this.disable = true
      this.fetchNightsong()
    },

    /**
     * A 10s cooldown after refresh.
     * 
     * @return { Void }
     */
    refreshCooldown() {
      setTimeout(() => {
        this.disable = false
      }, 10000)
    },

    /**
     * Fetches the data from Warmane API.
     * 
     * @return { Void }
     */
    async fetchNightsong() {
      this.loading = true
      this.disable = true

      try {
        const res = await axios.get(`${this.cors}/${this.api}/guild/Nightsong/Lordaeron/summary`)
        this.leader = res.data.leader
        this.members = res.data.roster.map((val, index) => {
          return {
            index: index,
            name: val.name,
            online: val.online,
            level: val.level,
            gender: val.gender,
            race: val.race,
            class: val.class,
            achievementpoints: val.achievementpoints,
            professions: val.professions,
            loading: false,
            disabled: false,
          }
        })
        console.log(this.members)
        this.loading = false
        this.refreshing = false
        this.refreshCooldown()        

        console.log(res.data)
      } catch (err) {
        this.loading = false
        this.refreshing = false

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
    fetchCharacter(name, index) {
      console.log(this.members[index])
      this.members[index].loading = true
      this.members.forEach(val => {
        val.disabled = true
      })

      setTimeout(() => {
        axios.get(`${this.cors}/${this.api}/character/${name}/Lordaeron/`)
        .then(res => {
          this.character = res.data
          this.characterInfo = true
          this.members[index].loading = false
          this.members.forEach(val => {
            val.disabled = false
          })

          console.log(res.data)
        })
        .catch(err => {
          this.members[index].loading = false
          this.members.forEach(val => {
            val.disabled = false
          })
          console.log(err)
        })
      }, 1500)
    },
  },
}
</script>
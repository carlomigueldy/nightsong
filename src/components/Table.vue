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
          text
          @click="refreshData()"
          :disabled="disable"
          :loading="refreshing">
          <v-icon>mdi-refresh</v-icon>
        </v-btn>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="members"
        :search="search"
        item-key="id"
        sort-by="online"
        :sort-desc="true"
      >
        <template v-slot:item.class="{ item }">
          <img :src="classAvatar(item.class)" height="35">
        </template>
        <template v-slot:item.race="{ item }">
          <img :src="genderRace(item.gender, item.race)" height="35">
        </template>
        <template v-slot:item.online="{ item }">
          <v-chip :color="item.online ? 'green lighten-1' : 'blue-grey darken-1'">
            {{ item.online ? 'Online' : 'Offline' }}
          </v-chip>
        </template>
        <template v-slot:item.professions="{ item }">
          <span v-for="(profession, index) in item.professions.professions" :key="index">
            {{ profession.name }} ({{ profession.skill }})
          </span>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-btn 
            text
            :loading="item.loading"
            :disabled="item.disabled"
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
        text: 'Race',
        align: 'left',
        sortable: true,
        value: 'race',
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
     * @return { String }
     */
    classAvatar(class_name) {
      switch (class_name) {
        case 'Death Knight':
          return '/img/classes/death_knight.png'
        case 'Druid':
          return '/img/classes/druid.png'
        case 'Hunter':
          return '/img/classes/hunter.png'
        case 'Mage':
          return '/img/classes/mage.png'
        case 'Paladin':
          return '/img/classes/paladin.png'
        case 'Priest':
          return '/img/classes/priest.png'
        case 'Rogue':
          return '/img/classes/rogue.png'
        case 'Shaman':
          return '/img/classes/shaman.png'
        case 'Warlock':
          return '/img/classes/warlock.png'
        case 'Warrior':
          return '/img/classes/warrior.png'
      }
    },

    /**
     * Evaluates the given parameter to 
     * identify their race and gender.
     * 
     * @param { String } gender
     * @param { String } race
     * @return { String } 
     */
    genderRace(gender, race) {
      if (gender === 'Male') {
        switch (race) {
          case 'Draenei':
            return '/img/races/draenei_male.png'
          case 'Dwarf':
            return '/img/races/dwarf_male.png'
          case 'Gnome':
            return '/img/races/gnome_male.png'
          case 'Human':
            return '/img/races/human_male.png'
          case 'Night Elf':
            return '/img/races/nightelf_male.png'
        } 
      } else if (gender === 'Female') {
        switch (race) {
          case 'Draenei':
            return '/img/races/draenei_female.png'
          case 'Dwarf':
            return '/img/races/dwarf_female.png'
          case 'Gnome':
            return '/img/races/gnome_female.png'
          case 'Human':
            return '/img/races/human_female.png'
          case 'Night Elf':
            return '/img/races/nightelf_female.png'
        } 
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
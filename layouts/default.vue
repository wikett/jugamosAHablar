<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
    >
      <v-list class="pa-1">
        <v-list-tile avatar>
          <v-list-tile-avatar>
            <img src="https://randomuser.me/api/portraits/men/85.jpg">
          </v-list-tile-avatar>

          <v-list-tile-content>
            <v-list-tile-title>John Leider</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>

      <v-list class="pt-0" dense>
        <v-divider></v-divider>

        <v-list-tile
          v-for="item in items"
          :key="item.title"
        >
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>

          <v-list-tile-content>
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar
      fixed
      app
    >
      <v-toolbar-side-icon @click="drawer = !drawer" />
      
      <v-toolbar-title v-text="title"/>
      
    </v-toolbar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    
    <v-footer
    dark
    height="auto"
  >
    <v-card
      class="flex"
      flat
      tile
    >
      <v-card-title class="teal">
        <strong class="subheading">Publicidad</strong>

        <v-spacer></v-spacer>
      </v-card-title>

      <v-card-actions class="grey darken-3 justify-left texto-footer">
        &copy;2019
      </v-card-actions>
    </v-card>
  </v-footer>
  </v-app>
</template>

<script>
  export default {
    data() {
      return {
        
        drawer: null,
        items: [
          { title: 'Home', icon: 'dashboard' },
          { title: 'About', icon: 'question_answer' }
        ],
        title: '¿Jugamos a Hablar?'
        
      }
    }
  }
</script>
<style lang="scss" scoped>

.texto-footer{
  font-size: 1.2rem;
}
</style>

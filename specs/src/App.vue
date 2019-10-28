<template>
  <v-app>
    <v-navigation-drawer
      app
      v-model="drawer"
    >
      <v-list dense>
        <v-list-item to="/">
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item v-for="packet in packets" :key="packet.version" :to="'/Release/'+packet.version">
          <v-list-item-action>
            <v-icon>mdi-file-cabinet</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Release {{packet.version}}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

    </v-navigation-drawer>
    <v-app-bar
      app
      color="blue"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-content>
          <router-view/>
    </v-content>
    <v-footer
      color="blue"
      app
    >
      <span class="white--text">&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import axios from 'axios' 

export default Vue.extend({
  name: 'App',
  components: {
  },
  data: () => ({
    drawer: null,
    packets: []
  }),
  mounted: function() {
    axios.get('/packets.json').then(response => {
      this.packets = response.data
    })
  }
});
</script>

<template>
  <div class="">
    <v-tabs
      fixed-tabs
      background-color="blue"
      dark
    >
      <v-tab>
        Produkttypen
      </v-tab>
      <v-tab>
        Dokumente
      </v-tab>


      <v-tab-item>
        <v-card>
          <v-card-title>
            Produkttypen
            <v-spacer></v-spacer>
            <v-text-field
              v-model="searchProductType"
              append-icon="mdi-magnify"
              label="Suche"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :headers="productTypeHeaders"
            :items="packet.productTypes"
            :items-per-page=10
            class="elevation-1"
            :search="searchProductType"
          ></v-data-table>
        </v-card>
      </v-tab-item>

      <v-tab-item>
        <v-card>
          <v-card-title>
            Dokumente
            <v-spacer></v-spacer>
            <v-text-field
              v-model="searchDocument"
              append-icon="mdi-magnify"
              label="Suche"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :headers="documentHeaders"
            :items="packet.documents"
            :items-per-page=10
            class="elevation-1"
              :search="searchDocument"
            ></v-data-table>
        </v-card>
      </v-tab-item>
    </v-tabs>

  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import axios from 'axios' 

export default Vue.extend({
  name: 'Release',
  components: {
  },
  data: () => ({
    searchProductType: '',
    searchDocument: '',
    productTypeHeaders: [
      {
        text: 'Produkttyp',
        align: 'left',
        sortable: true,
        value: 'type.id',
      },
      { text: 'Version', value: 'version' },
    ],
    documentHeaders: [
      {
        text: 'Document',
        align: 'left',
        sortable: true,
        value: 'of.name',
      },
      { text: 'Version', value: 'version' },
    ],
    packets: [],
    packet: {},
  }),
  beforeRouteUpdate: function (to, from, next) {
    this.switchRelease(to.params.version)
    next()
  },
  mounted: function() {
    this.fetchData()
  },
  methods: {
    fetchData () {
      axios.get('/packets.json').then(response => {
        this.packets = response.data
        this.switchRelease(this.$route.params.version)
      })        
    },
    switchRelease (version: string) {
      for (let p of this.packets) {
        if ("version" in p && p["version"] == version) {
            this.packet = p
            break;
        }
      }

    }
  }
});
</script>

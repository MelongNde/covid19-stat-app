<template>
  <v-layout>
    <v-flex class="">
      <v-content>
        <v-card>
          <v-card-title>
            Corona virus daily stats
            <v-spacer></v-spacer>
            <v-text-field

              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :sort-by.sync="sortBy"
            :sort-desc.sync="sortDesc"
            class="elevation-1"
            loading loading-text="Loading... Please wait"
            :headers="headers"
            :items="summaries.data"
            :search="search"
          ></v-data-table>
        </v-card>
        <v-container>
            <h1>St🛑p C🦠vid-19 😷</h1>
            <v-col cols="12">
              <v-autocomplete
                v-model="values"
                :items="items"
                dense
                chips
                small-chips
                label="Select country"
                multiple
                solo
              ></v-autocomplete>
            </v-col>
            <l-map
              id="map"
              style="min-height: 100vh"
              :zoom="zoom"
              :center="center"
              :options="{zoomControl: false}"
            >
              <l-tile-layer :url="url"></l-tile-layer>
              <l-marker
              :key = "index"
              v-for="(d, index) in summaries.data"

                :lat-lng="latLng(d.countryInfo.lat, d.countryInfo.long)"
              >

                <l-icon
                  :icon-size="iconSize"
                  :icon-url="icon"
                >

                </l-icon>
               </l-marker>
            </l-map>
        </v-container>
      </v-content>

    </v-flex>
  </v-layout>
</template>

<script>
import L from 'leaflet';

import axios from 'axios'
import { LMap, LTileLayer, LMarker, LIcon } from 'vue2-leaflet';
import virus from '../assets/virus.png'

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LIcon
  },
  methods: {
    latLng: function(lat, lng){
      return L.latLng(lat, lng)
    }
  },
  data: function(){
    return {
      search: '',
      sortBy: 'fat',
      sortDesc: false,
      headers: [
        {
            text: 'Country',
            align: 'start',
            sortable: false,
            value: 'country',
        },
        {
            text: 'Today cases',
            align: 'start',
            sortable: false,
            value: 'todayCases',
        },
        {
            text: 'Cases',
            align: 'start',
            sortable: false,
            value: 'cases',
        },
        {
            text: 'Deaths',
            align: 'start',
            sortable: false,
            value: 'deaths',
        },
        {
            text: 'Recovered',
            align: 'start',
            sortable: false,
            value: 'recovered',
        },
      ],
      infos: [],
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      summaries: Array,
      items: [],
      values: ['foo', 'bar'],
      value: null,
      zoom: 4,
      center: [28, 3],
      icon: virus,
      iconSize: [20, 20]
    };
  },
  mounted:function(){
    axios.get('https://corona.lmao.ninja/countries')
      .then((r => {
        for(const i in r.data ){
          this.summaries[i] = r.data[i]
          this.infos[i] = r.data[i]
        }
        this.summaries = r
        console.log('Infos: ',this.infos)


      }))
  },



}
</script>

<style scoped>
#map{
  z-index: 0 !important;
  width: 100%;
  height: 100%;
}
</style>

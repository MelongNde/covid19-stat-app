<template>
  <div>
    <H1>News</H1>

    <v-container fluid>
      <!-- {{news}} -->
      <v-row dense>
        <v-col
          v-for="card in news"
          :key="card.id"
          :cols="card.flex"
        >
          <v-card>
            <v-img
              :src="card.urlToImage"
              class="white--text align-end"
              gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
              height="200px"
            >
              <v-card-title v-text="card.title">{{card.title}}</v-card-title>
            </v-img>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn icon>
                <v-icon>mdi-heart</v-icon>
              </v-btn>

              <v-btn icon>
                <v-icon>mdi-bookmark</v-icon>
              </v-btn>

              <v-btn icon>
                <v-icon>mdi-share-variant</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      news: Array,
      cards: [
        { title: 'Pre-fab homes', src: 'https://cdn.vuetifyjs.com/images/cards/house.jpg', flex: 12 },
        { title: 'Favorite road trips', src: 'https://cdn.vuetifyjs.com/images/cards/road.jpg', flex: 6 },
        { title: 'Best airlines', src: 'https://cdn.vuetifyjs.com/images/cards/plane.jpg', flex: 6 },
      ]
    }
  },
  mounted:function(){
    axios.get('http://newsapi.org/v2/everything?q=bitcoin&from=2020-03-16&sortBy=publishedAt&apiKey=4c025222f1f04f5eaa610899d9e1dc97')
      .then((r => {
        for(const i in r.articles ){
          this.news[i] = r.articles[i]
        }
        this.news = r
        console.log('Infos: ',this.news)


      }))
  },
}
</script>

<style>

</style>

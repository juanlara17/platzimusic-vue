<template lang="pug">
#app
  img(src='./assets/logo.png')
  h1 PlatziMusic
  select(v-model="selectCountry")
    option(v-for="country in countries" :value="country.value") {{ country.name }}
  spinner(v-show="loading")
  ul
    artist(v-for="artist in artists" :artist="artist" :key='artist.mbid')
</template>

<script>
import getArtists from './api/'
import spinner from './components/spinner.vue'
import artist from './components/artist.vue'
export default {
  components: {
    artist,
    spinner
  },
  methods: {
    refreshArtists() {
      const self = this
        this.loading = true
        this.artists = []
        getArtists(this.selectCountry)
          .then(function (artists){
            self.loading = false
            self.artists = artists
          })
    }
  },
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name : 'Argentina', value : 'argentina' },
        { name : 'Colombia', value : 'colombia' },
        { name : 'España', value : 'spain' }
      ],
      selectCountry: 'argentina',
      loading: true,
    };
  },
  mounted(){
    this.refreshArtists()
  },
  watch: {
    selectCountry() {
      this.refreshArtists()
    }
  },

}
</script>

<style lang='stylus'>
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>

<template>
  <div class="static-page-wrapper">
    <h1 class="text-h4 text-capitalize text-weight-bold ">{{$t('stats_title')}}</h1>
    <p>{{$t('profile_stats_text')}}</p>
   <q-scroll-area style="height: 65vh">
    <q-list   class="rounded-borders full-width" >


      <q-item  v-for="game in games" :key="game.id">
        <q-item-section avatar>
          <q-item-label lines="1">{{game.game_type | formatGameType}}</q-item-label>
          <q-item-label caption>{{game.start | formatDate}}</q-item-label>
        </q-item-section>
        <q-item-section>
          <img class="game-image"  :src="game.image" alt="">
        </q-item-section>
         <q-item-section side>
          <p class="no-margin" :class="[game.result ? 'text-positve' : 'text-red']">{{game.result ? `WIN + ${game.level.rating} rating` : `LOOSE`}}</p>
        </q-item-section>

        <q-item-section side>
          <q-icon  :name="game.result ? 'check' : 'close'" :color="game.result ? 'positive' : 'red'" />
        </q-item-section>
      </q-item>

    </q-list>
    </q-scroll-area>
  </div>
</template>
<script>
import { mapActions, mapGetters} from 'vuex'
import { date } from 'quasar'
export default {
  data () {
    return {
      games:[],


    }
  },
  async mounted() {
    const response_games = await this.$api.get('/api/game_history')
    this.games = response_games.data
    console.log(this.games)
  },
  filters:{
    formatDate(val){
      let timeStamp = new Date(val)
      let formattedString = date.formatDate(timeStamp, 'YYYY-MM-DD HH:mm')
      return formattedString
    },
    formatGameType(val){
      switch(val) {
        case 'puzzle_qr':
          return  'QR PUZZLE'
        case 'puzzle_image':
          return  'ART PUZZLE'

      }
    }
  },

  methods: {

  }
}
</script>
<style lang="sass">
.game-image
  width: 50px
  height: 50px
  object-fit: contain
  transition: all .2s ease-out
  &:hover
    width: 400px
    height: 400px


</style>

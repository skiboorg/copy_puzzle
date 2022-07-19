<template>
    <div class="static-page-wrapper">
      <h1 class="text-h4  text-weight-bold">{{$t('rating_title')}}</h1>
      <p>{{$t('rating_text')}}</p>
      <q-btn-toggle
      v-model="type"
      toggle-color="primary"
      class="q-mb-lg"
      :options="[
        {label: 'Rating', value: 'rating'},
        {label: 'WITHDRAW', value: 'money'}]"/>
       <q-scroll-area style="height: 65vh">
      <q-list v-if="type==='rating'"  class="full-width" >

      <q-item v-for="player in players" :key="player.id">
        <q-item-section avatar>
          <q-avatar>
            <img v-if="player.avatar" :src="player.avatar">
             <img v-else src="~assets/ava.png">
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label lines="1">{{player.nickname}}</q-item-label>
          <q-item-label caption lines="2">
            Games : {{player.games}}
          </q-item-label>
        </q-item-section>

        <q-item-section side >
          Rating : {{player.rating}}
        </q-item-section>
      </q-item>


    </q-list>
      <q-list v-else  class="full-width" >

      <q-item v-for="player in add_money_players" :key="player.id">
        <q-item-section avatar>
          <q-avatar>
            <img v-if="player.avatar" :src="player.avatar">
             <img v-else src="~assets/ava.png">
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label lines="1">{{player.nickname}}</q-item-label>
          <q-item-label caption lines="2">
            Games : {{player.games}}
          </q-item-label>
        </q-item-section>

        <q-item-section v-if="player.total_remove>0" side >
          Withdraw {{player.total_remove}}
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
      type:'rating',
      players:[],
      add_money_players:[],

    }
  },
  async mounted() {
    const response_players = await this.$api.get('/api/ratings')
    this.players = response_players.data
    const response_add_money_players = await this.$api.get('/api/add_money_ratings')
    this.add_money_players = response_add_money_players.data
    console.log(this.add_money_players)
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

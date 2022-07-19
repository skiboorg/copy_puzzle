<template>
  <q-layout view="hHh lpR fFf">
    <q-no-ssr>
      <q-header  class="bg-accent lt-md"   elevated>
        <q-toolbar>
          <img @click="$router.push('/game')" class="cursor-pointer" src="~assets/logo-h.png" alt="">
          <q-toolbar-title>
          </q-toolbar-title>
          <q-btn
            flat
            dense
            round
            icon="menu"
            aria-label="Menu"
            @click="leftDrawerOpen = !leftDrawerOpen"
          />

        </q-toolbar>
      </q-header>
    </q-no-ssr>
    <q-drawer
      v-model="leftDrawerOpen"
      side="right"
      :width="375"
      elevated
      content-class="left-menu flex column items-center justify-evenly bg-accent"    >
      <q-card v-if="$user.loggedIn" class="bg-grey-4"  >
          <q-item>
            <q-item-section avatar>
              <q-avatar>
                <img v-if="$user.user.avatar" :src="$user.user.avatar">
                <img v-else src="https://cdn.quasar.dev/img/boy-avatar.png">
              </q-avatar>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{$user.user.nickname}}</q-item-label>
              <q-item-label caption>
                {{$user.user.email}}
              </q-item-label>
            </q-item-section>
          </q-item>
          <q-separator />
          <q-card-section >
            <p class="q-mb-xs">Balance: {{$user.user.balance + $user.user.add_balance}}</p>
            <p class="no-margin">Rating: {{$user.user.rating}}</p>
          </q-card-section>
        </q-card>
       <q-list  dark>
            <q-item clickable to="/game">
              <q-item-section avatar >
                <!--          <q-avatar rounded size="sm">-->
                <!--            <img src="https://cdn.quasar.dev/img/mountains.jpg">-->
                <!--          </q-avatar>-->
                <q-icon color="white" name="west" />
              </q-item-section>
              <q-item-section>{{$t('menu_back')}}</q-item-section>
            </q-item>
            <q-item clickable to="/lk/profile">
              <q-item-section avatar>
                <q-icon color="white" name="manage_accounts" />
              </q-item-section>
              <q-item-section>{{$t('menu_settings')}}</q-item-section>
            </q-item>
            <q-item clickable to="/lk/stats">
              <q-item-section avatar>
                <q-icon color="white" name="bar_chart" />
              </q-item-section>
              <q-item-section>{{$t('menu_stats')}}</q-item-section>
            </q-item>
            <q-item clickable to="/lk/balance">
              <q-item-section avatar>
                <q-icon color="white" name="account_balance_wallet" />
              </q-item-section>
              <q-item-section>{{$t('menu_balance')}}</q-item-section>
            </q-item>
            <q-item clickable @click="logoutUser">
              <q-item-section avatar>

                <q-icon color="white" name="logout" />
              </q-item-section>
              <q-item-section>{{$t('menu_logout')}}</q-item-section>
            </q-item>

          </q-list>
    </q-drawer>

    <q-page-container>
      <q-page class="row">
        <div class="col-2 left-menu flex column items-center justify-around gt-sm">
          <img @click="$router.push('/game')" class="cursor-pointer" src="~assets/logo.png" alt="">
          <q-list style="margin-bottom: 100px" dark>
            <q-item clickable to="/game">
              <q-item-section avatar >
                <!--          <q-avatar rounded size="sm">-->
                <!--            <img src="https://cdn.quasar.dev/img/mountains.jpg">-->
                <!--          </q-avatar>-->
                <q-icon color="white" name="west" />
              </q-item-section>
              <q-item-section>{{$t('menu_back')}}</q-item-section>
            </q-item>
            <q-item clickable to="/lk/profile">
              <q-item-section avatar>
                <q-icon color="white" name="manage_accounts" />
              </q-item-section>
              <q-item-section>{{$t('menu_settings')}}</q-item-section>
            </q-item>
            <q-item clickable to="/lk/stats">
              <q-item-section avatar>
                <q-icon color="white" name="bar_chart" />
              </q-item-section>
              <q-item-section>{{$t('menu_stats')}}</q-item-section>
            </q-item>
            <q-item clickable to="/lk/balance">
              <q-item-section avatar>
                <q-icon color="white" name="account_balance_wallet" />
              </q-item-section>
              <q-item-section>{{$t('menu_balance')}}</q-item-section>
            </q-item>
            <q-item clickable @click="logoutUser">
              <q-item-section avatar>

                <q-icon color="white" name="logout" />
              </q-item-section>
              <q-item-section>{{$t('menu_logout')}}</q-item-section>
            </q-item>

          </q-list>

        </div>
        <div class="container col-lg-10 col-md-9 col-sm-12 col-xs-12">
          <router-view />
        </div>
      </q-page>

    </q-page-container>
  </q-layout>
</template>

<script>

import { mapActions, mapGetters} from 'vuex'

export default {
  name: 'MainLayout',
  data () {
    return {
      leftDrawerOpen: false,

    }
  },
  methods:{
    ...mapActions('auth',['logoutUser'])

  }

}
</script>
<style lang="sass">
.header
  position: absolute
//& .q-toolbar
   //position: absolute

.left-menu
  background: $accent url(~assets/menu-bg.png) no-repeat center
  background-size: cover !important
  height: 100vh


</style>

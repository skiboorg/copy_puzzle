<template>
  <q-layout view="hHh lpR fFf" class="fullscreen" style="user-select: none">
    <q-no-ssr>
      <q-header  class="bg-accent lt-lg"   elevated>
        <q-toolbar>
          <img v-if="$user.loggedIn" @click="$router.push('/game')" class="cursor-pointer" src="~assets/logo-h.png" alt="">
          <img v-else @click="$router.push('/enter')" class="cursor-pointer" src="~assets/logo-h.png" alt="">
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
      overlay
      side="right"
      :width="375"
      elevated
      content-class="left-menu flex column items-center justify-evenly bg-accent">
      <q-toolbar class="absolute-top">
      <q-space/>
      <q-btn flat round dense color="white" @click="leftDrawerOpen=!leftDrawerOpen">
        <q-icon name="close" />
      </q-btn>
    </q-toolbar>

      <q-card v-if="$user.loggedIn" class="user-card"  >
            <q-item>
              <q-item-section avatar>
                <q-avatar>
                  <img v-if="$user.user.avatar" :src="$user.user.avatar">
                  <img v-else src="~assets/ava.png">
                </q-avatar>
              </q-item-section>
              <q-item-section>
                <q-item-label>{{$user.user.nickname}}</q-item-label>
                <q-item-label caption class="text-white">
                  {{$user.user.email}}
                </q-item-label>
              </q-item-section>
            </q-item>
            <q-separator />
            <q-card-section >
             <p class="q-mb-xs flex items-center">{{$t('menu_balance')}}: {{$user.user.balance + $user.user.add_balance}}
             <span class="inline-block q-ml-xs text-yellow text-bold">PZ</span>
             </p>
              <p class="no-margin">{{$t('menu_rating')}}: {{$user.user.rating}} out of 200</p>
            </q-card-section>
          </q-card>
      <q-list  dark>
        <!--v-if="$user.loggedIn"-->
        <q-item v-if="$user.loggedIn" clickable to="/lk/profile">
          <q-item-section avatar>
            <q-icon color="white" name="chat" />
          </q-item-section>
          <q-item-section>{{$t('menu_profile')}}</q-item-section>
        </q-item>
        <q-item v-if="!$user.loggedIn" clickable to="/enter">
          <q-item-section avatar>
            <q-icon color="white" name="login" />
          </q-item-section>
          <q-item-section>{{$t('menu_login')}}</q-item-section>
        </q-item>
        <!--        v-else-->
        <q-item v-if="$user.loggedIn" clickable to="rating">
          <q-item-section avatar>
            <q-icon color="white" name="bar_chart" />
          </q-item-section>
          <q-item-section>{{$t('menu_rating')}}</q-item-section>
        </q-item>
        <q-item clickable to="/about">
          <q-item-section avatar>
            <q-icon color="white" name="help_outline" />
          </q-item-section>
          <q-item-section>{{$t('menu_about')}}</q-item-section>
        </q-item>
         <q-item clickable to="/rules">
          <q-item-section avatar>

            <q-icon color="white" name="school" />
          </q-item-section>
          <q-item-section>{{$t('menu_rules')}}</q-item-section>
        </q-item>
        <q-item clickable to="info">
          <q-item-section avatar>
            <q-icon color="white" name="info" />
          </q-item-section>
          <q-item-section>{{$t('menu_info')}}</q-item-section>
        </q-item>
        <q-item clickable to="/feedback">
          <q-item-section avatar>
            <q-icon color="white" name="feedback" />
          </q-item-section>
          <q-item-section>{{$t('menu_feedback')}}</q-item-section>
        </q-item>

<!--        <q-item clickable to="/documents">-->
<!--          <q-item-section avatar>-->
<!--            <q-icon color="white" name="description" />-->
<!--          </q-item-section>-->
<!--          <q-item-section class="text-uppercase">{{$t('documents_title')}}</q-item-section>-->
<!--        </q-item>-->

        <q-item clickable to="/contacts">
          <q-item-section avatar>

            <q-icon color="white" name="contacts" />
          </q-item-section>
          <q-item-section>{{$t('menu_contacts')}}</q-item-section>
        </q-item>

<!--        <q-separator spaced="md" dark/>-->
<!--        <q-btn-dropdown flat unelevated color="white" icon="language" :label="$t('menu_language')">-->
<!--          <q-list>-->
<!--            <q-item clickable v-close-popup @click="lang='en-us'">-->
<!--              <q-item-section>-->
<!--                <q-item-label>English</q-item-label>-->
<!--              </q-item-section>-->
<!--            </q-item>-->

<!--            <q-item clickable v-close-popup @click="lang='cn'">-->
<!--              <q-item-section>-->
<!--                <q-item-label>{{$t('menu_lang_ch')}}</q-item-label>-->
<!--              </q-item-section>-->
<!--            </q-item>-->


<!--          </q-list>-->
<!--        </q-btn-dropdown>-->

      </q-list>
    </q-drawer>
    <q-page-container>
      <q-page class="row">
        <div id="left_menu" class="col-2 left-menu flex column items-center justify-around gt-md">
         <img v-if="$user.loggedIn" @click="$router.push('/game')" class="cursor-pointer" src="~assets/logo.png" alt="">
          <img v-else @click="$router.push('/enter')" class="cursor-pointer" src="~assets/logo.png" alt="">
          <q-card v-if="$user.loggedIn" class="user-card"  >
            <q-item>
              <q-item-section avatar>
                <q-avatar>
                  <img v-if="$user.user.avatar" :src="$user.user.avatar">
                  <img v-else src="~assets/ava.png">
                </q-avatar>
              </q-item-section>
              <q-item-section>
                <q-item-label>{{$user.user.nickname}}</q-item-label>
                <q-item-label caption class="text-white">
                  {{$user.user.email}}
                </q-item-label>
              </q-item-section>
            </q-item>
            <q-separator />
            <q-card-section >
              <p class="q-mb-xs flex items-center">{{$t('menu_balance')}}: {{$user.user.balance + $user.user.add_balance}}
                <span class="inline-block q-ml-xs text-yellow text-bold">PZ</span>
              </p>
              <p class="no-margin ">{{$t('menu_rating')}}: {{$user.user.rating}} out of 200</p>
            </q-card-section>
          </q-card>
         <q-list style="margin-bottom: 100px"  dark>
        <!--v-if="$user.loggedIn"-->
        <q-item v-if="$user.loggedIn" clickable to="/lk/profile">
          <q-item-section avatar>
            <q-icon color="white" name="chat" />
          </q-item-section>
          <q-item-section>{{$t('menu_profile')}}</q-item-section>
        </q-item>
        <q-item v-if="!$user.loggedIn" clickable to="/enter">
          <q-item-section avatar>
            <q-icon color="white" name="login" />
          </q-item-section>
          <q-item-section>{{$t('menu_login')}}</q-item-section>
        </q-item>
        <!--        v-else-->
        <q-item v-if="$user.loggedIn" clickable to="rating">
          <q-item-section avatar>
            <q-icon color="white" name="bar_chart" />
          </q-item-section>
          <q-item-section>{{$t('menu_rating')}}</q-item-section>
        </q-item>
        <q-item clickable to="/about">
          <q-item-section avatar>
            <q-icon color="white" name="help_outline" />
          </q-item-section>
          <q-item-section>{{$t('menu_about')}}</q-item-section>
        </q-item>
         <q-item clickable to="/rules">
          <q-item-section avatar>

            <q-icon color="white" name="school" />
          </q-item-section>
          <q-item-section>{{$t('menu_rules')}}</q-item-section>
        </q-item>
        <q-item clickable to="info">
          <q-item-section avatar>
            <q-icon color="white" name="info" />
          </q-item-section>
          <q-item-section>{{$t('menu_info')}}</q-item-section>
        </q-item>
        <q-item clickable to="/feedback">
          <q-item-section avatar>

            <q-icon color="white" name="feedback" />
          </q-item-section>
          <q-item-section>{{$t('menu_feedback')}}</q-item-section>
        </q-item>
<!--         <q-item clickable to="/documents">-->
<!--          <q-item-section avatar>-->
<!--            <q-icon color="white" name="description" />-->
<!--          </q-item-section>-->
<!--          <q-item-section class="text-uppercase">{{$t('documents_title')}}</q-item-section>-->
<!--        </q-item>-->
        <q-item clickable to="/contacts">
          <q-item-section avatar>

            <q-icon color="white" name="contacts" />
          </q-item-section>
          <q-item-section>{{$t('menu_contacts')}}</q-item-section>
        </q-item>

<!--        <q-separator spaced="md" dark/>-->
<!--        <q-btn-dropdown flat unelevated color="white" icon="language" :label="$t('menu_language')">-->
<!--          <q-list>-->
<!--            <q-item clickable v-close-popup @click="lang='en-us'">-->
<!--              <q-item-section>-->
<!--                <q-item-label>English</q-item-label>-->
<!--              </q-item-section>-->
<!--            </q-item>-->

<!--            <q-item clickable v-close-popup @click="lang='cn'">-->
<!--              <q-item-section>-->
<!--                <q-item-label>{{$t('menu_lang_ch')}}</q-item-label>-->
<!--              </q-item-section>-->
<!--            </q-item>-->


<!--          </q-list>-->
<!--        </q-btn-dropdown>-->

      </q-list>

        </div>
        <div class="container col-lg-10 col-md-12 col-sm-12 col-xs-12">
          <router-view />
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { mapActions, mapGetters} from 'vuex'
export default {
  data () {
    return {
      leftDrawerOpen:false,
      lang: this.$i18n.locale,
      langOptions: [
        { value: 'en-us', label: 'English' },
        { value: 'cn', label: 'China' }
      ]

    }
  },
  watch: {
    lang(lang) {
      this.$i18n.locale = lang
      this.$q.cookies.set('lang',lang)
    }
  },
  methods: {

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

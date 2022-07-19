<template>
  <div class="static-page-wrapper">
    <h1 class="text-h4  text-weight-bold">{{$t('feedback_title')}}</h1>
    <p class="text-grey-6">{{$t('feedback_text')}}</p>

    <q-scroll-area class="q-mb-lg" style="height: 50vh">
    <q-card class="q-mx-xs q-mb-md"  v-for="feedback in feedbacks" :key="feedback.id">
      <q-item>
          <q-item-section avatar>
            <q-avatar>
              <img v-if="feedback.user.avatar" :src="feedback.user.avatar">
               <img v-else src="~assets/ava.png">
            </q-avatar>
          </q-item-section>
        <q-item-section>
            <q-item-label>{{feedback.user.nickname}}</q-item-label>
            <q-item-label caption lines="1">{{feedback.user.email}}</q-item-label>
          </q-item-section>
        <q-item-section side >
          <q-item-label caption>{{feedback.date |formatDate}}</q-item-label>
        </q-item-section>

      </q-item>
        <q-separator/>
      <q-card-section >
        {{feedback.text}}
     </q-card-section>
      <q-card-section horizontal class="justify-between">
        <q-item>
          <q-item-section v-if="feedback.image" thumbnail>
            <img @click="curImage=feedback.image,imgModal=true" class="cursor-pointer" style="object-fit: contain"  :src="feedback.image">
          </q-item-section>
        </q-item>
      </q-card-section>
    </q-card>

    </q-scroll-area>
    <q-form v-if="$user.loggedIn" class="q-gutter-sm" @submit="formSubmit">
   <q-input
          filled
          v-model="message"
          name="message"
          :label="$t('write_message')"
          type="textarea"
          autogrow
          lazy-rules
          :rules="[ val => val && val.length > 0 || 'Please type something']"
        />

      <div class="flex items-center justify-between">
        <q-file
        name="image"
        dense
        v-model="image"
        style="flex-basis: 20%"
        outlined
        color="primary"
        :label="$t('contacts_form_attach')"
      />
        <q-btn size="md" :label="$t('contacts_form_send')" type="submit" color="primary"/>

      </div>
    </q-form>
    <q-dialog v-model="imgModal">
      <q-card style="width: 700px; max-width: 80vw;">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6"></div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section>
          <q-img :ratio="1" :src="curImage"/>
       </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>
<script>
import { date } from 'quasar'
export default {
  data () {
    return {
      feedbacks:[],
      message:null,
      image:null,
      curImage:'',
      imgModal:false
    }
  },
  async mounted() {
    const response_feedbacks = await this.$api.get('/api/all_fb')
    this.feedbacks = response_feedbacks.data
    console.log(this.feedbacks)
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
    async formSubmit(evt){
      console.log('submit')
      const formData = new FormData(evt.target)
      const response = await this.$api.post('/api/add_fb',formData)
      this.$q.notify({
          message: 'Message sent',
          position: this.$q.screen.lt.sm ? 'bottom' : 'bottom-right',
          color: 'positive',
          icon: 'announcement'
        })
      this.message=''
      this.image=null
      console.log(response)
    }
  }
}
</script>

<template>
  <div class="static-page-wrapper">
    <h1 class="text-h4 text-weight-bold">Контакты</h1>
    <p class="text-grey-8" v-html="$t('contacts_info')"></p>

    <h3 class="text-h4 text-weight-bold">{{$t('contacts_form_title')}}</h3>
    <p class="text-grey-8">{{$t('contacts_form_text')}}</p>
    <q-form  class="q-gutter-sm"  @submit="formSubmit"  >
      <div class="form-grid ">
        <q-input
          class="col-6"
          filled
          v-model="email"
          :label="$t('email')"

          lazy-rules
          :rules="[ val => val && val.length > 0 || 'Please type something']"
        />
            <q-input
        filled
        v-model="name"
        :label="$t('name')"

        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type something']"
      />

      </div>
   <q-input
          filled
          v-model="message"
          class="col-6"
          :label="$t('write_message')"
          type="textarea"
          lazy-rules
          :rules="[ val => val && val.length > 0 || 'Please type something']"
        />



      <div>
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

        <q-btn :label="$t('contacts_form_send')" type="submit" color="primary"/>

        </div>

      </div>
    </q-form>
  </div>
</template>
<script>


export default {
  name: 'MainLayout',

  data () {
    return {
      image:null,
      name:'',
      email:'',
      message:'',

    }
  },
  methods:{
    formSubmit(){
      this.$q.notify({
          message: 'Message sent',
          position: this.$q.screen.lt.sm ? 'bottom' : 'bottom-right',
          color: 'positive',
          icon: 'announcement'
        })
      this.$user.loggedIn ? this.$router.push('/game') : this.$router.push('/auth')
    }
  }

}
</script>
<style lang="sass">
.form-grid
  display: grid
  grid-template-columns: 1fr 1fr
  grid-gap: 20px
</style>

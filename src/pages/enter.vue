<template>
  <div class="full-height flex column items-center justify-center">
    <p class="text-h6 text-bold">{{is_register ? $t('register_btn'):$t('login_btn') }}</p>
    <q-form
      v-if="is_register"
      @submit="onSubmit"
      style="width: 320px"
      class="q-gutter-sm form">

      <q-input
      filled
      :dense="!$q.screen.gt.md"
      v-model="userData.email"
      :label="$t('email')"
      lazy-rules
      :rules="[ val => val && val.length > 0 || 'Please type email',
      val => email_re.test(String(val)) || 'Please type correct email'
      ]"/>
      <q-input
      filled
      :dense="!$q.screen.gt.md"
      v-model="userData.nickname"
      :label="$t('nickname')"
      lazy-rules
      :rules="[ val => val && val.length > 0 || 'Please type something']"/>
      <q-input
      filled
      :dense="!$q.screen.gt.md"
      v-model="userData.age"
      type="number"
      :label="$t('age')"
      lazy-rules
      :rules="[ val => val && val.length > 0 || 'Please type your age']"/>
      <q-select class="q-mb-lg" filled :dense="!$q.screen.gt.md" v-model="userData.sex" :options="sex_options" label="Sex" />


      <q-input
      filled
      :dense="!$q.screen.gt.md"
      v-model="userData.password1"
      :label="$t('password')"
      :type="isPwd ? 'password' : 'text'"
      lazy-rules
      :rules="[ val => val && val.length >= 4|| 'Password must at least 4 letters']">
        <template v-slot:append>
          <q-icon
            :name="isPwd ? 'visibility_off' : 'visibility'"
            class="cursor-pointer"
            @click="isPwd = !isPwd"
          />
        </template>
      </q-input>
      <q-input
      filled
      :dense="!$q.screen.gt.md"
      v-model="userData.password2"
      :type="isPwd ? 'password' : 'text'"
      :label="$t('password_repeat')"
      lazy-rules
      :rules="[ val => val && val===this.userData.password1 || 'Passwords not match']">
        <template v-slot:append>
          <q-icon
            :name="isPwd ? 'visibility_off' : 'visibility'"
            class="cursor-pointer"
            @click="isPwd = !isPwd"
          />
        </template>
      </q-input>

      <div class="flex items-center">
         <q-toggle v-model="accept"  />
        <p class="no-margin">I accept the <a href="/contract_offer.docx">license and terms</a></p>
      </div>
      <div class="flex items-center">
         <q-toggle v-model="accept1"  />
        <p class="no-margin">I accept the <a href="/user_agreement.docx">user agreement</a></p>
      </div>
      <div class="flex items-center">
         <q-toggle v-model="accept2"  />
        <p class="no-margin">I accept the <a href="/privacy_policy.docx">privacy policy</a></p>
      </div>



      <div>
        <div class="text-center">
          <q-btn :label="$t('register_btn')" :disable="!accept || !accept1 || !accept2" type="submit" class="q-px-xl q-mb-md" color="primary "/>
          <p>{{$t('have_account')}} <a href="#" @click.prevent="is_register=false">{{$t('sign_in')}}</a></p>
        </div>


      </div>
    </q-form>
    <q-form v-else @submit="userLoginAction" class=" q-gutter-sd q-mb-lg">
      <q-input
        filled
        :dense="!$q.screen.gt.md"
        v-model="userLogin.email"
        :label="$t('email')+ '*'"

        style="width: 320px"
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Введите email']"
      />
      <q-input
        :dense="!$q.screen.gt.md"
        filled
        :type="isPwd ? 'password' : 'text'"
        v-model="userLogin.password"
        :label="$t('password')+ '*'"
        lazy-rules
        :rules="[val => val !== null && val !== '' || 'Введите пароль' ]">
        <template v-slot:append>
          <q-icon
            :name="isPwd ? 'visibility_off' : 'visibility'"
            class="cursor-pointer"
            @click="isPwd = !isPwd"
          />
        </template>
      </q-input>
      <div class="text-center">
        <q-btn size="md" :label="$t('login_btn')" type="submit" color="primary" class="q-px-xl q-mb-md"/>
      <p>{{$t('no_account')}} <a class="text-primary" href="#" @click.prevent="is_register=true">{{$t('sign_up')}}</a></p>
      <p>{{$t('lost_password')}} <router-link class="text-primary" to="contacts">{{$t('contact_us')}}</router-link> </p>

        <p @click="$router.push('/game')" class="text-primary flex items-center justify-center cursor-pointer">
          <q-icon size="2rem" name="shutter_speed" class="q-mr-md" /> {{$t('try_demo')}}
        </p>
      </div>

    </q-form>


  </div>


</template>

<script>
import { mapActions, mapGetters} from 'vuex'
export default {
  data () {
    return {
      name: null,
      age: null,
      accept: false,
      accept1: false,
      accept2: false,
      is_register:false,
      isPwd: true,
      lang: this.$i18n.locale,
      email_re:/^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
      userLogin:{
        email:'1@1.11',
        password:'123',
      },
      sex_options:[
        'Male', 'Female'
      ],
      userData:{
       password1:null,
       password2:null,
        email:null,
        age:null,
        sex:'Male',
        nickname:null,
      }

    }
  },
  methods: {
    ... mapActions('auth',['loginUser','logoutUser']),
    userLoginAction() {
      this.loginUser(this.userLogin)

    },
    onSubmit () {
      console.log('submit')
        this.completeRegistration()
    },
    async completeRegistration() {
      try {
        let response = await this.$api.post('/auth/users/', {
          nickname: this.userData.nickname,
          email: this.userData.email,
          age: this.userData.age,
          sex: this.userData.sex,
          password: this.userData.password2,
        })
        this.$q.notify({
          message: 'Аккаунт создан',
          position: this.$q.screen.lt.sm ? 'bottom' : 'bottom-right',
          color: 'positive',
          icon: 'announcement'
        })
        this.loading = false
        this.is_register = false
      } catch (e) {
        this.$q.notify({
          message: 'Проверьте введеные данные',
          position: this.$q.screen.lt.sm ? 'bottom' : 'bottom-right',
          color: 'red',
          icon: 'announcement'
        })
      }
    }
  }
}
</script>
<style lang="sass">

.form
  width: 400px

</style>

<template>
    <div class="static-page-wrapper">
      <h1 class="text-h4  text-weight-bold">{{$t('balance_title')}}</h1>
      <p class="text-grey-6" v-html="$t('balance_text')"></p>
      <div class="flex items-start">
         <q-btn-toggle
      v-model="pay_type"
      toggle-color="primary"
      class="q-mb-lg q-mr-lg"
      :options="[
        {label: $t('withwraw_button'), value: 'WITHDRAW'},
        {label: $t('deposit_button'), value: 'DEPOSIT'}]"/>
        <p class="q-mb-none  payment-text">Payments are accepted only from china</p>
      </div>

      <p class="text-grey-6">ATTENTION! 1 PUZ COIN = 1 YUAN</p>
      <q-form v-if="pay_type==='WITHDRAW'" @submit="formWithDrawSubmit" class=" q-gutter-sd q-mb-lg">
          <q-input
            filled
            v-model="remove_amount"
            :label="`${$t('amount')} * (max. ${$user.user.add_balance})`"
            type="number"
            max="10000"
            style="width: 320px"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Input amount',
             val => val <= $user.user.add_balance || `Можно снять ${$user.user.add_balance} или меньше` ,
             ]"
          />
        <q-option-group
      :options="pay_options"
      label="Notifications"
      type="radio"
      v-model="pay_system"
    />

          <q-btn size="md" :label="$t('withwraw_button')" type="submit" color="primary" class="q-my-sm "/>
        </q-form>
        <q-form v-else @submit="formDepositSubmit" class=" q-gutter-sd q-mb-lg">
          <q-input
            filled
            v-model="add_amount"
            :label="$t('amount')"
            type="number"
            style="width: 320px"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Input amount']"
          />
        <q-option-group
      :options="pay_options"
      label="Notifications"
      type="radio"
      v-model="pay_system"
    />

          <q-btn size="md" :label="$t('deposit_button')" type="submit" color="primary" class="q-my-sm "/>
        </q-form>
    </div>
</template>
<script>




import {mapActions} from "vuex";

export default {
  name: 'MainLayout',

  data () {
    return {
      pay_type:'WITHDRAW',
      pay_system:'bank_card',
      add_amount:null,
      remove_amount:null,
      pay_options: [
        { label: 'Bank Сard', value: 'bank_card' },
        { label: 'AliPay', value: 'alipay' },
        { label: 'WeChatPay', value: 'wechat' },

      ]
    }
  },
  methods:{
    ... mapActions('auth',['getUser']),
    async formWithDrawSubmit(){
      console.log('submit')
      await this.$api.post('/api/user/remove_money',{amount:this.remove_amount})
      await this.getUser()

      this.$q.notify({
          message: 'Operation complete',
          position: this.$q.screen.lt.sm ? 'bottom' : 'bottom-right',
          color: 'positive',
          icon: 'announcement'
        })

    },
    async formDepositSubmit(){
      console.log('submit')
      await this.$api.post('/api/user/add_money',{amount:this.add_amount})
      await this.getUser()

      this.$q.notify({
          message: 'Operation complete',
          position: this.$q.screen.lt.sm ? 'bottom' : 'bottom-right',
          color: 'positive',
          icon: 'announcement'
        })
    },
  }

}
</script>
<style lang="sass">
.payment-text
  border: 2px solid red
  padding: 5px 10px
  color: red
</style>

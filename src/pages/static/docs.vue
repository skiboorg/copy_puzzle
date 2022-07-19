<template>
     <div class="static-page-wrapper">
      <h1 class="text-h4  text-weight-bold">{{$t('documents_title')}}</h1>
       <q-scroll-area style="height: 70vh; max-width: 100%;">

         <div class="flex items-start ">
           <div class="">
               <p class="q-mb-lg">
公司地址：上海市松江区荣乐东路301号<br>
阿尤布（Ayub）电话：13262863703<br>
我们公司的微信：<br>
商业执照：<br>
银行：ICBC中国工ր银行<br>
帐号：1001261809207951247<br>
帐户持有人：AYOUB AKHEMASSI<br>
公司：上海二迩信息科技有限公司<br>
实体办公室地址：上海市松江区广富林路4855弄7号楼<br>

    </p>
         <div class="q-mb-lg">
           <p class="text-bold ">Our main documents:</p>
         <a class="doc-link q-mr-md q-mb-md" href="/contract_offer.docx">license and terms</a>
         <a class="doc-link q-mr-md q-mb-md" href="/user_agreement.docx">user agreement</a>
           <a class="doc-link" href="/privacy_policy.docx">privacy policy</a>
         </div>
           </div>
           <div style="flex-basis: 50%" class="text-center ">
             <img src="~assets/QR.png" alt="">
           </div>
         </div>









    </q-scroll-area>

    </div>
</template>

<script>
export default {
  name: 'Error404'
}
</script>
<style lang="sass">
.doc-link
  color: inherit
  display: inline-block
  text-decoration: none
  text-transform: uppercase
  border: 1px solid black
  padding: 10px 15px
  font-weight: bold
</style>

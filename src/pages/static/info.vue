<template>
    <div class="static-page-wrapper">
      <h1 class="text-h4  text-weight-bold">{{$t('info_title')}}</h1>
       <q-scroll-area style="height: 70vh; max-width: 100%;">

        <p v-html="$t('infoText')"></p>

    </q-scroll-area>

    </div>
</template>

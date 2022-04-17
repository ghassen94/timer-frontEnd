<template>
  <div v-bind:class=" periodIndexProp+periodeContainerStyle > 1 ? 'defaultPeriodeContainerStyle flex between-h light-gray-bottom-border' : 'defaultPeriodeContainerStyle flex between-h '+periodeContainerStyle">
      <div class="left-text">
        <DefaultText v-bind:text="'Period '+periodIndexProp" v-bind:textStyle="periodStyleProp" />
        <DefaultText v-if="isCurrent" text="Current Period" textStyle="current-period-txt-style initial-display" />
      </div>

      <div class="flex">
        <DefaultText v-bind:text=" isCurrent ? 'Started '+startAt : startAt+' - '+endAt" textStyle="begin-end-txt-style margin-auto m-r32" />
        <TimeText v-bind:hour="hh" v-bind:minutes="mm" v-bind:seconds="ss" v-bind:textStyle="'time-style margin-auto '+timeTextStyleProps" v-bind:secondesStyle="'weight-300 robotoFontLight time-style '+secondesStyleProps"/>
      </div>
  </div>
</template>
<script lang = "ts" >
  import { defineComponent } from 'vue';
  import DefaultText from './DefaultText.vue'
import TimeText from './TimeText.vue'
  
  export default defineComponent({
    name: 'PeriodItem',
    components: {
      DefaultText,
      TimeText,
    },
    props: {
      // period item Props
      periodeContainerStyle: {
        type: String,
        default: "defaultPeriodeContainerStyle"
      },

      // periode props
      periodIndexProp: {
        type: String,
        default: "0"
      },
      
      periodStyleProp: {
        type: String,
        default: "default-text-style"
      },

      // current props
      isCurrent : {
        type: Boolean,
        default: false
      },

      // start end time
      startAt : {
        type: String,
        default:""
      },
      endAt : {
        type: String,
        default:""
      },

      // TimeText props
      hh: {
        type: Number,
        default: 0
      },
      mm: {
        type: Number,
        default: 0
      },
      ss: {
        type: Number,
        default: 0
      },
      timeTextStyleProps: {
        type: String,
        default: ""
      },
      secondesStyleProps: {
        type: String,
        default: "default-text-style robotoFontLight"
      },
    }
  });
</script>
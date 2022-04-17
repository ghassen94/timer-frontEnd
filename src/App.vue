<template>
  <PeriodItem text="12:09 - 12:09 pm" textStyle="begin-end-txt-style" /> 
  <DefaultButton v-bind:withIcon="true" btn-text="Button text" :icon-src="require('@/assets/images/clock.svg')" />
  <DefaultText text="Today, 08 Oct" textStyle="today-txt-style" />
  <DefaultText text="Period 3" textStyle="period-number-txt-style" />
  <DefaultText text="Current Period" textStyle="current-period-txt-style" />
  <DefaultText text="12:09 - 12:09 pm" textStyle="begin-end-txt-style" />
  <DefaultText text="12:09 - 12:09 pm" textStyle="begin-end-txt-style green-text" />
  <TimeText hour="01" minutes="24" seconds="54" textStyle="time-style" secondesStyle="weight-300 robotoFontLight"/>
  <TimeText hour="01" minutes="24" seconds="54" textStyle="time-style green-text" secondesStyle="weight-300 robotoFontLight"/>
  <DefaultText text="Total" textStyle="total-time-txt-style" /> 
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import DefaultButton from './components/uikit/DefaultButton.vue'
import DefaultText from './components/uikit/DefaultText.vue'
import TimeText from './components/uikit/TimeText.vue'
import PeriodItem from './components/uikit/PeriodItem.vue'
  
export default defineComponent({
  name: 'App',
  data() {
    return {
      
    }
  },
  components: {
    DefaultButton,
    DefaultText,
    TimeText,
    PeriodItem
  }
});
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    padding: 10% 20%;
  }

  .total-container {
    display: flex;
    justify-content: flex-end;
    padding: 14px 64px;
    border-top: 1px solid $light-gray-color;
    border-bottom: 1px solid $light-gray-color;
    margin-top: 100px;
  }

  .m-r40 {
    margin-right: 40px;
  }

  .m-t20 {
    margin-top: 20px;
  }

  .green-boder-top-bottom {
    border-top: 1px solid $green-color !important;
    border-bottom: 1px solid $green-color;
  }

  .margin-auto {
    margin: auto;
  }

  .m-b97 {
    margin-bottom: 97px !important;
  }

  .m-b0 {
    margin-bottom: 0px !important;
  }

  .m-r13 {
    margin-right: 13px !important;
  }
  
  .m-r32 {
    margin-right: 32px !important;
  }

  .flex  {
    display: flex;
  }

  .initial-display {
    display: initial;
  }

  .between-h {
    justify-content: space-between;
  }

  .weight-300 {
    font-weight: 300 !important;
  }

  .robotoFontLight {
    font-family: $robotoFontLight !important;
  }

  .red-text {
    color: $red-color !important;
  }

  .red-border {
    border-color: $red-color !important;
  }

  .green-text {
    color: $green-color !important;
  }

  .left-text {
    text-align: left;
  }

  .right-text {
    text-align: right;
  }

  .green-border {
    border-color: $green-color !important;
  }

  .no-bottom-border {
    border-bottom: none;
  }

  .today-txt-style {
    color: $gray-color-level-5;
    font-family: $robotoFont;
    font-style: normal;
    font-weight: 900;
    font-size: 30px;
    line-height: 35px;
  }

  .period-number-txt-style {
    color: $gray-color-level-4;
    font-family: $robotoFont;
    font-style: normal;
    font-weight: 900;
    font-size: 16px;
    line-height: 19px;
  }

  .current-period-txt-style {
    color: $gray-color-level-1;
    font-family: $robotoFont;
    font-style: normal;
    font-weight: 900;
    font-size: 16px;
    line-height: 19px;
  }

  .begin-end-txt-style {
    color: $gray-color-level-3;
    font-family: $robotoFontMeduim;
    font-style: normal;
    font-weight: 900;
    font-size: 14px;
    line-height: 16px;
  }

  .time-style {
    color: $gray-color-level-5;
    font-family: $robotoFontMeduim;
    font-style: normal;
    font-weight: 900;
    font-size: 25px;
    line-height: 29px;
  }

  .total-time-txt-style {
    color: $gray-color-level-2;
    font-family: $robotoFont;
    font-style: normal;
    font-weight: 900;
    font-size: 25px;
    line-height: 29px;
  }

  // Period item 
  .defaultPeriodeContainerStyle {
      padding: 23px 64px;
      border-top: 1px solid $light-gray-color;
  }

  .light-gray-bottom-border{
    border-bottom: 1px solid $light-gray-color;
  }

   .period-number-txt-style {
    color: $gray-color-level-4;
    font-family: $robotoFont;
    font-style: normal;
    font-weight: 900;
    font-size: 16px;
    line-height: 19px;
  }
  // Period item 

</style>

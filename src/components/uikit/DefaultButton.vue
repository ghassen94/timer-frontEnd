<template>
  <button v-on:click="action" v-bind:class="'default-button '+btnStyle">
    <img v-if="withIcon" v-bind:src="iconSrc" v-bind:class="iconStyle" />
    {{ btnText }}
  </button>
</template>

<script lang = "ts" >
  import { defineComponent } from 'vue';
  export default defineComponent({
    name: 'DefaultButton',
    props: {
      iconSrc:String,
      withIcon:Boolean,
      iconStyle: {
        type: String,
        default: "default-button-icon"
      },
      btnStyle: {
        type: String,
        default: "default-button"
      },
      btnText: String,
      action: Function,
    }
  });
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  .default-button-icon {
    width:13px;
    height:15.89px;
    margin-right: 10px;
  }

  .default-button {
    border: 1px solid $black-color;
    box-sizing: border-box;
    border-radius: 5px;
    background-color: $transparent;
    padding: 8px 9px 8px 12px;
    color: $black-color;
    font-family: $robotoFont;
    display: flex;
    align-items: center;
  }

</style>

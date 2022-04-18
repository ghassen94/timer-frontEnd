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

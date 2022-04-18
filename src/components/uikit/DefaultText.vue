<template>
  <p v-bind:class="textStyle">
    {{ text }}
  </p>
</template>

<script lang = "ts" >
  import { defineComponent } from 'vue';
  export default defineComponent({
    name: 'DefaultText',
    props: {
      text: {
        type: String,
        default: "default-text"
      },
      textStyle: {
        type: String,
        default: "default-text-style"
      },
    }
  });
</script>
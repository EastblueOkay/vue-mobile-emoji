<template>
  <div id="app">

    <!-- vue-mobile-emoji -->
    <vme @emojiSelected="emojiSected"></vme>
    <!-- vue-mobile-emoji -->


    <br/>
    <!-- show demo -->
    <h3 style="text-align: center;"
        contenteditable="true"
        ref="demo"
    >
      vue-mobile-emoji (可编辑)
    </h3>
    <!-- show demo -->

  </div>
</template>

<script>

  import vme from "./components/vue-mobile-emoji/vme.vue"

  export default {
    name: 'app',
    components: {
      vme
    },
    methods: {
      emojiSected(emoji, img) {
        const dom = this.$refs.demo.innerHTML + `<img src="${img}" />`;
        this.$refs.demo.innerHTML = dom;
      }
    }
  }

</script>

<style>
  #app {
    margin: 0;
    padding: 0;
  }

  #app h3 {
    font-size: 20px;
    font-family: 500;
  }

  #app h3 img {
    width: 20px;
    height: 20px;
  }
</style>

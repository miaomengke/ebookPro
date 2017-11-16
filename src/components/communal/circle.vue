<template>
  <i-circle :percent="percent"
            :trail-width="6"
            :stroke-width="7"
            stroke-color="#ffcc62">
    <span class="demo-Circle-inner" style="font-size:24px">{{percent}}%</span>
  </i-circle>
</template>
<script>
//  import Velocity from 'velocity-animate'
  // import 'iview/dist/styles/iview.css'
  export default {
    data () {
      return {
      }
    },
    computed: {
    },
    methods: {
    },
    props: ['percent']
  }
</script>
<style>
</style>

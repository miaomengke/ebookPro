<template>
  <div class="articleTemp" v-swipe="gotoAddress">
    <div class="article"  v-html="currentArticle.article"></div>
  </div>
</template>
<script>
//  import Vue from 'vue'
  import {mapState, mapMutations} from 'vuex'
//  import {vueTouch} from '../../../js/interface'
//  Vue.directive('swipe', {
//    bind: function (el, binding) {
//      vueTouch(el, binding, 'swipe')
//    }
//  })
  export default {
    name: 'articleTemp',
    data () {
      return {
      }
    },
    components: {
    },
    props: ['currentArticle'],
    methods: {
      ...mapMutations(['CHANGE_ARTICLE', 'IS_KEEPEYE']),
      gotoAddress (e) {
      },
      keepEye () {
        this.IS_KEEPEYE()
      }
    },
    computed: {
      ...mapState([
        'isKeepEye'
      ])
    }
  }
</script>
<style>
  .articleTemp_top{
    display: flex;
    justify-content: flex-end;
  }
  .keep_eye_box{
    margin: 2rem;
  }
  .keep_eye_box .keep_eye_span{
    color:#0b97c4;
    padding: 1rem 2rem;
    border: solid 1px #0b97c4;
    border-radius: 01rem;
  }
  .articleTemp{
    padding: 2rem 2.5rem 9.2rem;
    font-size: 40px;
    /*text-indent:2em;*/
    font-family: SimSun;
    line-height: 5rem;
    /*letter-spacing: 0.4rem ;*/
  }
  .article h1, .article h2 {
    padding-top: 2rem;
    display: flex;
    justify-content: center;
  }
  .article span{
    padding-top: 1rem;
    display: flex;
    justify-content: center;
  }
  .article h1{
  }
  .text_introduces{
    padding-top: 2rem;
    font-size: 2.5rem;
    color: #707070;
  }
  .article span sup, h1 sup{
    top: 0.5rem;
    color: #00A000;
  }
  .article h1{
    font-size: 4rem;
  }
  .article h2{
    font-size: 3rem;
    color: #707070;
  }
  .article .annotation{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }
  .annotation span{
    display: flex;
    justify-content: flex-start;
  }
</style>

<template>
  <div class="guide" @click="addPath(userInfo? '/articles':'/loginAndRegist')">
    <!--<section class="content">-->
      <!--<div class="guide_img"  @click="hideGuide"><p>LOGO</p></div>-->
      <!--<div class="guide_title">群文阅读</div>-->
      <!--<div class="guide_text">读书改变生活</div>-->
    <!--</section>-->
    <section class="content">
      <div class="img" :style="{backgroundImage: 'url('+ guideImg +')'}"></div>
    </section>
  </div>
</template>
<script>
  export default {
    name: 'guide',
    path: '',
    data () {
      return {
        guideImg: require('../../images/guide.gif'),
        userInfo: false
      }
    },
    methods: {
      addPath (path) {
        this.$router.push(path)
      },
      hideGuide () {
        if (this.userInfo) {
          this.path = '/articles'
        } else {
          this.path = '/loginAndRegist'
        }
        var self = this
        setTimeout(function () {
          self.$router.push(self.path)
        }, 3000)
      }
    },
    mounted: function () {
      this.hideGuide()
    }
  }
</script>
<style lang="scss" scoped>
  @import "../../style/mixin.scss";
  .guide{
  @include allcover();
    width: 100%;
    height: 100%;
    z-index: 9999;
    background-color: #fff;
    display: flex;
    justify-content: center;
  }
  .content{
    margin: 0;
    padding: 0;
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    font-size: 4rem;
    color: #7f7f7f;
  }
  .img{
    height: 100%;
    width: 100%;
    margin: auto;
    background: no-repeat;
    background-size:100% 100%;
  }
  .content div:nth-child(2){
    margin: 3rem;
    font-size: 5rem;
    color: #000;
  }
  .guide_img{
    height: 20rem;
    width: 20rem;
    background-color: #0b97c4;
    border-radius: 50%;
    display: flex;
    justify-content: center;
 }
  .guide_img p{
    line-height: 12rem;
    color: #ffffff;
  }
</style>

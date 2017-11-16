<template>
  <div class="readGroupArticle" @touthMove="touthMove">
    <!--顶部导航-->
    <head-top go-back='true' title='true'  :head-title="headTitle"></head-top>
    <!--正文-->
    <article-temp :article="article"></article-temp>
    <!--<transition :name="transitionName" >-->
    <!--<router-view class="child-view"></router-view>-->
    <!--</transition>-->
    <!--<main class="article">-->
    <!--<div class="swiper-container">-->
    <!--<div class="swiper-wrapper">-->
    <!--<div class="swiper-slide">Slide 1</div>-->
    <!--<div class="swiper-slide">Slide 2</div>-->
    <!--<div class="swiper-slide">Slide 3</div>-->
    <!--</div>-->
    <!--&lt;!&ndash; 如果需要分页器 &ndash;&gt;-->
    <!--<div class="swiper-pagination"></div>-->
    <!--</div>-->
    <!--</main>-->
  </div>
</template>
<script>
  import headTop from '../../communal/header/head'
  import articleTemp from '../../home/readGroupArticle/articleTemp'
  import Swiper from 'swiper'
  import '../../../style/swiper.min.css'
  export default {
    data () {
      return {
        headTitle: '《小半》',
        eMove: '',
        backPath: '/textIntroduces',
        transitionName: '',
        isTouch: false,
        article: '《小半》别闹好好学《小半》别闹好好学《小小半》别闹好好学《小半》别闹好好学《小半》小半》别闹好好学《小半》别闹好好学《小半》小半》别闹好好学《小半》别闹好好学《小半》小半》别闹好好学《小半》别闹好好学《小半》小半》别闹好好学《小半》别闹好好学《小半》小半》别闹好好学《小半》别闹好好学《小半》小半》别闹好好学《小半》别闹好好学《小半》半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹好好学《小半》别闹'
      }
    },
    methods: {
      addPath (path) {
        this.$router.push(path)
//        this.$router.replace(path)
      },
      handleScroll () {
        var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
        console.log(scrollTop)
      },
      touthMove () {
        var scrollTop = window.pageXOffset
        console.log(scrollTop)
      }
    },
    components: {
      headTop,
      articleTemp
    },
    mounted () {
      // 初始化swiper
      var mySwiper = new Swiper('.swiper-container', {
        speed: 600,
        onTouchEnd: function () {
          mySwiper.startAutoplay()
        }
      })
//      this.eMove = this.isTouch ? 'touchmove' : 'mousemove'
//      window.addEventListener('scroll', this.handleScroll)
//      window.addEventListener(this.eMove, this.touthMove)
    },
    watch: {
      '$route' (to, from) {
        const toDepth = to.path.split('/').length
        const fromDepth = from.path.split('/').length
        this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
      }
    }
  }
</script>
<style lang="scss" scoped>
  @import "../../../style/mixin.scss";
  .readGroupArticle{
    padding-top: 5rem;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
  .article{
    background-color: #7c5e53;
    height: 100%;
  }
  .swiper-container{
    height: 100%;
  }
  /*.article{*/
  /*background-color: #7c5e53;*/
  /*!*height: 90%;*!*/
  /*}.swiper-slide{*/
  /*height: 60rem;*/
  /*}*/
  /* 上面是为了保证滑动的时候不出现抖动情况 */
  .child-view {
    position: absolute;
    left:0;
    top: 0;
    height: 100%;
    width: 100%;
    transition: all .5s cubic-bezier(.55,0,.1,1);
    background-color: #f2f2f2;
    overflow: auto;
    -webkit-overflow-scrolling: touch;
  }
  /* 当child-view的内容过多时会撑开child-view使得内部能够滚动 */
  .slide-left-enter, .slide-right-leave-active {
    opacity: 0;
    -webkit-transform: translate(750px, 0);
    transform: translate(750px, 0);
    transition-delay: .5s;
    -webkit-transition-delay: .5s;
  }
  .slide-left-leave-active, .slide-right-enter {
    opacity: 0;
    -webkit-transform: translate(-750px, 0);
    transform: translate(-750px, 0);
    transition-delay: .5s;
    -webkit-transition-delay: .5s;
  }
  .slide-enter-active {
    -webkit-transition: all .3s ease;
    transition: all .3s ease;
  }
  .slide-leave-active {
    -webkit-transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  /* 然后写上切换时候的类名的CSS变化(这里最好看一下VUE的transition文档) */
</style>

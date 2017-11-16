<template>
  <div >
    <section id='foot_guid'>
      <section v-for="(item, index) in links" @click="gotoAddress(item.route)"
               class="guide_item "
               :style="{backgroundImage:$route.path.indexOf(item.path)!== -1? 'url('+ item.guideItemActImg +')':'url('+ item.guideItemImg +')'}"
               :class="{'active':$route.path.indexOf(item.path)!== -1}">
        <div class="guide_item_div">
        </div>
      </section>
      <!--<section @click="gotoAddress(links[0].route)" class="guide_item " :class="{'active':$route.path.indexOf('articles')!== -1}">-->
        <!--<div class="guide_item_div">-->
          <!--&lt;!&ndash;<div class="img_box"><p></p></div>&ndash;&gt;-->
          <!--&lt;!&ndash;<p>阅读文</p>&ndash;&gt;-->
        <!--</div>-->
      <!--</section>-->
      <!--<section @click="gotoAddress(links[1].route)"  class="guide_item" :class="{'active':$route.path.indexOf('readingCircle')!== -1}">-->
        <!--<div class="guide_item_div">-->
          <!--&lt;!&ndash;<div class="img_box"><p></p></div>&ndash;&gt;-->
          <!--&lt;!&ndash;<p>阅读圈</p>&ndash;&gt;-->
        <!--</div>-->
      <!--</section>-->
      <!--<section @click="gotoAddress(links[2].route)"  class="guide_item" :class="{'active':$route.path.indexOf('my')!== -1}">-->
        <!--<div class="guide_item_div">-->
          <!--&lt;!&ndash;<div class="img_box"><p></p></div>&ndash;&gt;-->
          <!--&lt;!&ndash;<p>我的</p>&ndash;&gt;-->
        <!--</div>-->
      <!--</section>-->
    </section>
  </div>
</template>
<script>
  export default {
    name: 'foot_guide',
    data () {
      return {
        footBarShow: true,
        links: [
          {
            isActive: true,
            text: '阅读文',
            route: '/articles',
            path: 'articles',
            guideItemImg: require('../../../images/icon/Article.png'),
            guideItemActImg: require('../../../images/icon/Article_yes.png')
          },
          {
            isActive: false,
            text: '阅读圈',
            route: '/readingCircle',
            path: 'readingCircle',
            guideItemImg: require('../../../images/icon/Readers.png'),
            guideItemActImg: require('../../../images/icon/Readers_yes.png')
          },
          {
            isActive: false,
            text: '我的',
            route: '/myInfo',
            path: 'myInfo',
            guideItemImg: require('../../../images/icon/my.png'),
            guideItemActImg: require('../../../images/icon/my_yes.png')
          }
        ]
      }
    },
    props: ['footShow'],
    mounted () {

    },
    components: {

    },
    computed: {

    },
    methods: {
      gotoAddress (path) {
        this.$router.push(path)
      }
    }
  }
</script>

<style>
  #foot_guid{
    background-color:#ffffff;
    /*background: url("../../../images/icon/menu_bar.png") ;*/
    background-size:100% 100%;
    position: fixed;
    z-index: 100;
    height: 110px;
    left: 0;
    right: 0;
    bottom: 0;
    font-size: 2rem;
    display: flex;
    border-top: solid 1px #cccccc;
  }
  .guide_item{
    flex: 1;
  }
  .guide_item{
  background: no-repeat center ;
  background-size: 100px 100px;
  }
</style>

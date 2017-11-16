<template>
  <div>
    <!--顶部导航-->
    <head-top go-back='' title='true'  headTitle=悦读圈 message ='true'></head-top>
    <div id="content" class="topH">
      <div id="firstArea">
        <span id="xuebaSpan">学霸推荐</span>
        <span class="right" @click="randomData">换一批</span>
      </div>
     
      <div id="bookArea">
        <transition-group enter-active-class="flipInY" leave-active-class="flipOutY" @before-enter="beforeenter" @before-leave="beforeleave">
          <div class="titleC1 animated"  v-show="show" v-if="index<3" v-for="(item, index) in items" :key="index" @click="classmateInfo(item.ID)">
            <div id="imgDiv">
              <img :src="item.bookSrc" id="bookImg"  alt="logo">  
            </div>
            <p v-text="item.userName"></p>
            <p v-text="item.bookName" :class="bookName"></p>
          </div>
        </transition-group>
      </div>

    </div>
    <section id="tab-container">
      <!-- <router-link id="publicActive" to="">发布动态</router-link> -->
      <ul id="tab-nav" class="text-center spacing" style="padding: 0">
          <li v-for="(item, index) in links" ><a :class="item.className" @click="changeUrl(index, item)">{{item.text}}</a></li>
      </ul>
      <div class="tab-content">
        <transition enter-active-class="animated bounceInLeft">
          <router-view></router-view>
        </transition>
      </div>
      
    </section>
    
    <!--底部导航-->
    <foot-guide></foot-guide>
  </div>
</template>
<script>
  import './../../style/animate.css'
  import headTop from '../communal/header/head'
  import footGuide from '../communal/footer/foot'
  export default {
    data () {
      return {
        bookName: 'bookName',
        show: true,
        links: [
          {
            text: '热门动态',
            name: 'page01A',
            className: '',
            route: '/readingCircle/page01-a'
          },
          {
            text: '本班动态',
            name: 'page01B',
            className: '',
            route: '/readingCircle/page01-b'
          }
        ],
        items: [
          {
            bookSrc: require('./../../assets/personImg/Avatar photos1.jpg'),
            userName: '李梅',
            bookName: '背影',
            ID: '24454568'
          },
          {
            bookSrc: require('./../../assets/personImg/Avatar photos2.jpg'),
            userName: '王大虎',
            bookName: '阿Q正传',
            ID: '24454569'
          },
          {
            bookSrc: require('./../../assets/personImg/Avatar photos3.jpg'),
            userName: '张晓雅',
            bookName: '雷锋日记',
            ID: '24454570'
          },
          {
            bookSrc: require('./../../assets/personImg/Avatar photos4.jpg'),
            userName: '米娅',
            bookName: '水浒传未删版',
            ID: '24454571'
          },
          {
            bookSrc: require('./../../assets/personImg/Avatar photos5.jpg'),
            userName: '艾琳',
            bookName: '梁山伯与祝英台',
            ID: '24454572'
          }
        ]
      }
    },
    components: {
      headTop,
      footGuide
    },
    methods: {
      changeUrl (index, item) {
        // if (index === 0) {
        //   document.getElementById('tab-nav').childNodes[1].className = ''
        //   document.getElementById('tab-nav').childNodes[0].className = 'activeEffect'
        // } else {
        //   document.getElementById('tab-nav').childNodes[0].className = ''
        //   document.getElementById('tab-nav').childNodes[1].className = 'activeEffect'
        // }
        this.$router.push(item.route)
        let arr = item.route.split('/')
        let pageName = arr[arr.length - 1]
        if (pageName === 'page01-a') {
          this.links[0].className = 'activeEffect'
          this.links[1].className = ''
        } else if (pageName === 'page01-b') {
          this.links[0].className = ''
          this.links[1].className = 'activeEffect'
        }
        // console.log(arr)
      },
      randomData () {
        this.show = !this.show
        this.items = this.items.sort(randomsort)
      },
      classmateInfo (studentID) {
        this.$router.push({name: 'classmateInfo', params: {studentID: studentID}})
      },
      beforeleave (el) {
        // console.log('动画结束')
        this.show = true
      },
      beforeenter (el) {
        // console.log('动画开始')
        this.show = true
      }
    },
    created: function () {
      let arr = this.$route.path.split('/')
      let pageName = arr[arr.length - 1]
      if (pageName === 'page01-a') {
        this.links[0].className = 'activeEffect'
        this.links[1].className = ''
      } else if (pageName === 'page01-b') {
        this.links[0].className = ''
        this.links[1].className = 'activeEffect'
      }
      // console.log(pageName)
    }
  }
  function randomsort (a, b) {
    // 用Math.random()函数生成0~1之间的随机数与0.5比较，返回-1或1
    return Math.random() > 0.5 ? -1 : 1
  }
</script>
<style>
body{
  background: #EEEEEE;
  font-family: '微软雅黑'
}
.topH{
  margin-top: 7.3rem;
}
    #firstArea{
      width: 100%;
      padding: 20px 0px;
      text-align: left;
      border-bottom:1px solid rgba(0,0,0,.3);
    }
.bookName{
   font-size: 32px;
   color: rgba(0,0,0,.5);
}
#bookArea{
  display: flex;
  flex-direction: row;
  justify-content:space-around;
  background:white;
  padding: 20px 35px;
  border-bottom:2px solid rgba(0,0,0,.3);
}
#bookArea span{
  display: flex;
  direction: row;
  width: 100%;
  justify-content: space-between;
}
#content #firstArea #xuebaSpan{
  height: 40px;
  font-size: 40px;
  line-height: 40px;
  margin-left: 40px;
  padding-left: 20px;
  border-left: 10px solid #1A9FFF;
}
#firstArea span.right{
  display:inline-block;
  text-align: right;
  position: absolute;
  right: 30px;
  padding-top: 4px;
  font-size: 35px;
  font-weight: bolder;
  color: rgba(26,159,255,.7);
}
  .titleC1{
    float: left;
    border:2px solid rgba(0,0,0,.2);
    font-size: 36px;
    text-align: left;
    border-radius: 10px;
  }
  #bookArea div #imgDiv{
    /*display: none;*/
    text-align: center;
    padding:5px;
  }
  .titleC1 #bookImg{
    display: inline-block;
    text-align: center;
    width: 210px;
    height: 210px;
  }
  #active{
    margin-top: 500px;
    width: 100%;
  }

  #tab-container{
    position: relative;
    top:2rem;
    background: rgba(0,0,0,.1);
  }

  #tab-container #publicActive{
    display: inline-block;
    position: absolute;
    text-decoration: none;
    width: 250px;
    height: 100px;
    line-height: 100px;
    font-size: 40px;
    text-align: center;
    right: 22px;
/*    top: 0px;*/
    color: white;
    z-index: 2;
    background: #1A9FFF;
    font-weight: bolder;
    border: 1px solid rgba(0,0,0,.3);
    box-shadow: 0px 1px 50px 10px rgba(0,0,0,.3);
  }

  #tab-container #tab-nav{
    /*background-color: #1Abc9C;*/
    border-top: 1px solid rgba(0,0,0,.3);
    border-bottom: 2px solid rgba(0,0,0,.3);
    box-shadow: 0px 1px 30px 3px rgba(0,0,0,.3);
    background: white;
    color: rgba(0,0,0,.5);
    font-weight: bolder;
  }
  #tab-container #tab-nav li{
    display: inline-flex;
    flex-direction: row;
    justify-content: flex-start;
    list-style: none;
    width: 25%;
    height: 100px;
    line-height: 100px;
    font-size: 36px;
    margin-bottom: 0px;
    text-align: center;
  }
  #tab-container #tab-nav li a{
    display: inline-block;
    height: 60px;
    line-height: 60px;
    margin: auto 0px;
    width: 100%;
    color: rgba(0,0,0,.5);
  }

  #tab-container #tab-nav li:first-child a{
    border-right: 4px solid rgba(0,0,0,.3);
  }

  .tab-content{
    width: 95%;
    margin: 3px auto;
    background: white;

  }
  .font40{
    font-size: 40px;
  }
 #tab-container #tab-nav li .activeEffect{
    color: #1A9FFF;
    /*background: #1A9FFF;*/
    /*border-bottom: 4px solid #444444;*/
  }
</style>

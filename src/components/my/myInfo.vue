<template>
  <div>
    <!--顶部导航-->
    <!-- <head-top signin-up='home'  go-back='' title='true'  headTitle='我的信息' message =''> -->
      <!--<span slot='msite-title' class="head_logo">阅读文</span>-->
    <!-- </head-top> -->
    
    <div id="content">
      <div id="content-title" :style="{background:'url('+bgImgSrc+') center center no-repeat',backgroundSize: '100% 100%'}">
        <img class="imgSize200" :src="imgSrc" @click="myInfo('/modify')">
        <h2 class="text-center">{{title}}</h2>
        <p class="description1">{{description1}}</p>
        <p class="description2">{{description2}}</p>
      </div>
      <div id="content-main">
      
        <table>
            <tr>
              <td class="first">积分</td>
              <td class="second" v-text = "score">0</td>
              <td class="three">排行榜</td>
              <td class="four" v-text = "rank">0</td>
              <td class="five">
                <p class="qiandao">
                  <label class="bg1" v-if="!checked">签到<input v-show="false" v-model="checked" type="checkbox" name="" value=""></label>
                  <label class="bg2" v-else>{{newSignTotalDay}}</label>
                </p>
              </td>
            </tr>
          </table>

        <div id="menuDiv">
          <ul>
            <li v-for="(item,index) in items" @click="itemRoute(index)">
              <span class="item1"><img :src="item.imgSrc"></span>
              <span class="item2">{{item.desc}}</span>
              <span class="item3">{{item.number}}</span>
              <span class="item4">{{item.jiantou}}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <!--底部导航-->
    <foot-guide></foot-guide>
  </div>
</template>

<script>
  import headTop from '../communal/header/head'
  import footGuide from '../communal/footer/foot'
  import store from '@/store/index'
  
  export default {
    name: 'my',
    data () {
      return {
        checked: false,
        headTitle: '我的信息',
        bgImgSrc: require('./../../assets/my/mybgm.jpg'),
        title: store.state.userInfo['24454565'].userName,
        imgSrc: store.state.userInfo['24454565'].logoImgSrc,
        description1: store.state.userInfo['24454565'].signature,
        description2: store.state.schoolInfo[store.state.userInfo['24454565'].schoolInfoId],
        signTotalDay: store.state.userInfo['24454565'].signTotalDay + 0,
        rank: store.state.userInfo['24454565'].rank + 0,
        score: store.state.userInfo['24454565'].score + 0,
        items: [
          {
            imgSrc: require('./../../assets/GeneralIcon/Heart.png'),
            desc: '我的收藏',
            number: '',
            jiantou: ''
          },
          {
            imgSrc: require('./../../assets/GeneralIcon/Magazine.png'),
            desc: '我的笔记',
            number: '',
            jiantou: ''
          },
          {
            imgSrc: require('./../../assets/GeneralIcon/Write.png'),
            desc: '我的提问',
            number: '',
            jiantou: ''
          },
          {
            imgSrc: require('./../../assets/GeneralIcon/Message.png'),
            desc: '我的消息',
            number: '05',
            jiantou: '>'
          },
          {
            imgSrc: require('./../../assets/GeneralIcon/Settings.png'),
            desc: '个人设置',
            number: '',
            jiantou: ''
          }
        ]
      }
    },
    components: {
      headTop,
      footGuide
    },
    computed: {
      newSignTotalDay: function () {
        return (++this.signTotalDay) + '天'
      }
    },
    methods: {
      myInfo (path) {
        this.$router.push(path)
      },
      itemRoute (index) {
        if (index === 0) {
          this.$router.push('/myCollection')
        } else if (index === 1) {
          this.$router.push('/myNotes')
        } else if (index === 2) {
          this.$router.push('/myQuestion')
        } else if (index === 3) {
          this.$router.push('/message')
        } else if (index === 4) {
          this.$router.push('/personalsSetting')
        }
      }
    },
    mounted: function () {
      // 计算我的收藏里面有没有数据
      // var userInfoObj = store.state.userInfo
      let bookListArr = store.state.bookList
      let computedNum = 0
      for (let i = 0; i < bookListArr.length; i++) {
        // 找出登录人收藏的书
        if (bookListArr[i].isCollect === true) {
          computedNum++
        }
      }
      if (computedNum !== 0) {
        this.items[0].number = computedNum < 10 ? '0' + computedNum : computedNum
        this.items[0].jiantou = '>'
      } else {
        this.items[0].number = ''
        this.items[0].jiantou = ''
      }
    }
  }
</script>

<style>
  #content {
    font-size:36px;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
/*    margin-top: 115px;*/
    background: #EEEEEE;
}
#content #content-title{
  width: 100%;
  height: 100%;
  /*background: url(./../../assets/my/mybgm.jpg) center center no-repeat;*/
  background-size: 100% 100%;
  padding-top: 4rem;
  padding-bottom: 1rem;
  color: white;
}
#content #content-main{
 padding: 30px;
}
.imgSize200{
  border-radius: 50%;
  width: 200px;
  height: 200px;
}
.imgSize100{
  border-radius: 50%;
  width: 120px;
  height: 120px;
}
.qiandao{
  height: 80px;
  width: 180px;
  font-size: 40px;
}
.qiandao .bg1{
  display: inline-block;
  width: 200px;
  height: 80px;
  background: url('./../../assets/GeneralIcon/Sign.png') no-repeat;
  background-size: 200px 80px;
  color: white;
  padding-top: 1rem;
  text-indent: -2.5rem;
}
.qiandao .bg2{
  display: inline-block;
  width: 200px;
  height: 80px;
  background: url('./../../assets/GeneralIcon/Signed in.png') no-repeat;
  background-size: 200px 80px;
  color: white;
  padding-top: 1rem;
  text-indent: -2.5rem;
}
.qiandao input{
  display: inline-block;
  position: absolute;
  top: -5px;
  right: -50px;
  border: 1px solid black;
  width: 50px;
  height: 50px;
  border-radius: 5px;
}
.text-center,.description2{
  margin: 0 0;
  padding-bottom: 30px;
}
.description2{
  /*color: #333333;*/
  margin-top: 10px;
  font-size: 32px;
  line-height: 25px;
}

#content table,tr, td{
  table-layout:fixed;
  border-collapse: collapse;
  font-size: 32px;
}
.last1{
  border-bottom: 1px solid rgba(0,0,0,0);
}

#content table{
  width:100%;
  height:100%;
}
#content table tr td.first{
  width:10%;
}
#content table tr td.second{
  width:10%;
  color: #6699ff;
}
#content table tr td.three{
  width:15%;
}
#content table tr td.four{
  width:15%;
  color: #6699ff;
}
#content table tr td.five{
  width:90%;
  display: inline-flex;
  flex-direction: row;
  justify-content: flex-end;
}
.last2 input{
    position: absolute;
    top: 626px;
    right: 100px;
    margin:50px auto;
    font-size:40px;
    width: 300px;
    background-color: #6699ff;
    border-radius: 10px;
    border: 0px;
    height: 80px;
    color: white;
    font-family: "微软雅黑"
 }
#content #menuDiv{
  background: white;
  margin-top: 30px;
  margin-bottom: 300px;
  box-shadow: 0px 3px 3px #999999;
}
#content ul li{
  display:block;
  height: 120px;
  border-bottom:1px solid #999999;
  display: flex;
  justify-content: space-around;
}

#content ul li span.item1{
  display: flex;
  align-items: center;
  width: 60px;
  height: 120px;
}
#content ul li span.item2{
  align-items: center;
  width: 200px;
  height: 120px;
  line-height: 120px;
}
#content ul li span.item3{
  align-items: center;
  width: 80px;
  height: 120px;
  line-height: 120px;
  margin-left: 500px; 
}
#content ul li span.item4{
  align-items: center;
  width: 60px;
  height: 120px;
  line-height: 120px;
}

#content ul li span.item1 img{
  width:60px;
  height:60px;
}
</style>

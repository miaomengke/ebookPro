<template>
  <div id="classmateInfo">
    <!--顶部导航-->
    <head-top signin-up='home'  go-back='true' title='true'  headTitle='同学信息' message =''>
      <!--<span slot='msite-title' class="head_logo">阅读文</span>-->
    </head-top>

    <div id="content" class="topH">
      <div id="classmateInfoTitleDiv" :style="{background:'url('+bgImgSrc+') center center no-repeat',backgroundSize: '100% 100%'}">
        <img :src="imgSrc" class="imgSize200">
        <h2 class="text-center">{{title}}</h2>
        <p class="description1">{{description1}}</p>
        <p class="description2">{{description2}}</p>
        <p>
          <span>收获的赞：</span>
          <span>{{this.goodNums}}</span>
          <span class="right1">积分：</span>
          <span class="right2">{{this.score}}</span>
        </p>
      </div>
        
      <section id="tab-container">
        <ul id="tab-nav" style="padding: 0">
            <li v-for="(item, index) in links" ><a :class="item.className" @click="changeUrl(index, item)" v-if="index<2">{{item.text}}</a></li>
        </ul>
        <div id="tab-content">
          <div v-show="show" id="noPannel">
            <p>
              <span>暂无</span>
              <span>{{this.msg}}</span>
              <span @click="sendMsg" class="effec">@他</span>
              <span>发布一条吧</span>
            </p>
          </div>
          <div id="answer-panel">
            <ul>
              <li v-for="(item,index) in items2" :key="items2.length-1">
              <div id="pgA-area1" class="top10">
                  <img class="imgSize100" :src="item.imgSrc"  alt="logo">      
              </div>

              <div id="pgA-area2">
                <p>
                  <span>{{item.userName}}</span>
                  <span class="right0">{{item.descTime}}</span>
                </p>
                <p><span class="center">{{item.yiti}}</span></p>
                <!-- <p>
                  <span class="font35 rgba5">评论数</span>
                  <span class="font35 rgba5">{{item.discussNum}}</span>
                  <span class="right1 font35 rgba5">点赞数</span>
                  <span class="right2 font35 rgba5">{{item.goodNum}}</span>
                </p>    --> 
              </div>
            </li>
            </ul>
          </div>
          <div id="question-panel">
            <ul>
              <li v-for="item in items1">
                <div id="pgA-area1">
                    <img class="imgSize150" :src="item.imgSrc"  alt="logo">      
                </div>
                <div id="pgA-area2">
                <p class="one">
                  <span class="fontSize2p7">{{item.userName}}</span>
                      <span class="right0 fontSize2p">{{item.descTime}}</span>
                </p>
                    <p class="two"><span class="center">{{item.yiti}}</span></p>
                    <p class="three">
                      <span class="fontSize2p">{{item.goodNum}}</span>
                      <span id="thumbsIcon" :style="{background:'url('+bgIcon2+') center center no-repeat',backgroundSize: '100% 100%'}"></span>
                      <span class="fontSize2p">{{item.discussNum}}</span>
                      <span id="chatIcon" :style="{background:'url('+bgIcon1+') center center no-repeat',backgroundSize: '100% 100%'}"></span>
                    </p>      
                </div>
              </li>
            </ul>
          </div>
        </div>
      </section>
    </div>
  <alert-tip v-if="showAlert" :showHide="showAlert" @closeTip="closeTip" :alertText="alertText"></alert-tip>
  </div>
    
</template>
<script>
  import headTop from '../../communal/header/head'
  import store from '@/store/index'
  import alertTip from '../../communal/alertTip'
  export default {
    name: 'my',
    data () {
      return {
        headTitle: '我的信息',
        title: '金姗姗',
        ID: '',
        show: false,
        msg: '',
        showAlert: false,
        bgIcon1: require('./../../../assets/GeneralIcon/Chat_yes.png'),
        bgIcon2: require('./../../../assets/GeneralIcon/thumbs.png'),
        imgSrc: require('./../../../assets/logo.png'),
        bgImgSrc: require('./../../../assets/my/mybgm.jpg'),
        description1: '我要学习优质的教学资源',
        description2: '深圳南山第一小学四年1班',
        goodNums: '200',
        score: '20',
        links: [
          {
            text: '他的动态',
            name: 'page01A',
            className: 'activeEffect',
            route: '/readingCircle/page01-a'
          },
          {
            text: '他的评论',
            name: 'page01B',
            route: '/readingCircle/page01-b'
          },
          {
          }
        ],
        items1: [
        ],
        items2: [
        ]
      }
    },
    components: {
      headTop, alertTip
    },
    methods: {
      changeUrl (index, item) {
        if (index === 0) {
          this.links[0].className = 'activeEffect'
          this.links[1].className = ''
          document.getElementById('answer-panel').style.display = 'none'
          if (this.items1.length === 0) {
            document.getElementById('question-panel').style.display = 'none'
            this.show = true
            this.msg = '动态'
          } else {
            document.getElementById('question-panel').style.display = 'block'
            this.show = false
          }
        } else {
          this.links[1].className = 'activeEffect'
          this.links[0].className = ''
          if (this.items2.length === 0) {
            document.getElementById('answer-panel').style.display = 'none'
            this.show = true
            this.msg = '评论'
          } else {
            document.getElementById('answer-panel').style.display = 'block'
            this.show = false
          }
          document.getElementById('question-panel').style.display = 'none'
        }
        // this.$goRoute(item.route)
      },
      classmateInfo (path) {
        this.$router.push(path)
      },
      closeTip () {
        this.showAlert = false
      },
      sendMsg () {
        this.showAlert = true
        this.alertText = '通知信息已經發送到對方收件箱'
      }
    },
    created: function () {
      this.ID = this.$route.params.studentID
      // console.log(this.$route.params)
      let publicActiveObj = store.state.publicActiveInfo
      let userInfoObj = store.state.userInfo[this.ID]
      this.imgSrc = userInfoObj.logoImgSrc
      this.title = userInfoObj.userName
      this.description1 = userInfoObj.signature
      this.description2 = store.state.schoolInfo[userInfoObj.schoolInfoId]
      this.score = userInfoObj.score
      this.goodNums = userInfoObj.rank
      for (let key in store.state.discussInfo) {
        let arr = store.state.discussInfo[key]
        for (let i = 0; i < arr.length; i++) {
          if (arr[i].hasOwnProperty('userInfoID') && this.ID === arr[i].userInfoID) {
            let tempObj1 = {}
            tempObj1['imgSrc'] = userInfoObj.logoImgSrc
            tempObj1['userName'] = userInfoObj.userName
            tempObj1['descTime'] = arr[i].time
            tempObj1['yiti'] = arr[i].theme
            this.items2.push(tempObj1)
          }
        }
      }

      for (let key in publicActiveObj) {
        let arr = publicActiveObj[key]
        for (let i = 0; i < arr.length; i++) {
          let tempObj = {}
          if (this.ID === key) {
            tempObj['imgSrc'] = userInfoObj.logoImgSrc
            tempObj['userName'] = userInfoObj.userName
            tempObj['descTime'] = arr[i].time
            tempObj['yiti'] = arr[i].theme
            tempObj['discussNum'] = arr[i].discussNum
            tempObj['goodNum'] = arr[i].goodNums
            tempObj['discussInfoId'] = arr[i].discussInfoId
            tempObj['publicActiveInfoId'] = arr[i].publicActiveInfoId
            tempObj['userInfoID'] = key
            tempObj['isThumbs'] = arr[i].isThumbs
            this.items1.push(tempObj)
          }
        }
      }
      if (this.items1.length === 0) {
        this.show = true
        this.msg = '動態'
      }
    }
  }
</script>
<style scoped>
  #classmateInfo{
    position: relative;
  }
  #classmateInfo #content #classmateInfoTitleDiv{
    width: 100%;
    height: 100%;
   /* background: url(./../../../assets/my/mybgm.jpg) center center no-repeat;
    background-size: 100% 100%;*/
    padding-top: 4rem;
    padding-bottom: 1rem;
    color: white;
    position: relative;
    z-index: 1;
  }
  #classmateInfo #content #tab-container{
    position: relative;
    top: 0px;
    /*margin-top: -32px;*/
  }  
  #classmateInfo #content #tab-container #tab-content #answer-panel{
    display: none;
  }
  #classmateInfo #content #tab-container #tab-nav{
    position: relative;
    text-align: left;
    box-shadow: none;
    background: #EEE;
    border:none;
    margin-top:-35px; 
    z-index: 0;
  }
  #classmateInfo #content #tab-container #tab-nav li{
    background: white;
    border: 2px solid rgba(0,0,0,.5);
    border-bottom: none;
  }
  #classmateInfo #content #tab-container #tab-nav li a{
    display: inline-block;
  }
   #classmateInfo #content #tab-container #tab-nav li:first-child{
    width: 30%;
    border-right: none;
   }
   #classmateInfo #content #tab-container #tab-nav li:nth-child(2){
    width: 30%;
    border-left: none;
   }
   #classmateInfo #content #tab-container #tab-nav li:nth-child(3){
    position:relative;
    top:36px;
    width: 39.5%;
    background: #EEE;
    border: none;
    border-bottom: 2px solid rgba(0,0,0,.5);
   }
    #classmateInfo #content #tab-container #tab-content{
      position: relative;
      height: 100%;
      background: white;
    }
    #classmateInfo #content #tab-container #tab-content ul li{
      height: 100%;
      background: white;
    }
    #tab-container #tab-nav li a.activeEffect{
      /*color:red;*/
      color: #1A9FFF;
    }
    #tab-content #noPannel span{
      font-size: 40px;
    }
    #tab-content #noPannel span.effec{
      color: green;
      
    }
    #tab-content #noPannel span.effec:hover{
      color: yellow;
      text-shadow: 1px 2px 1px black;
      font-weight: bolder;
    }
</style>

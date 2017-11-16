<template>
  <div id="box">
  	<ul>
      <li v-for="item in newItems" @click="discussInfo(item)">
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
              <span id="thumbsIcon" :style="{background:'url('+bgIcon2+') center center no-repeat',backgroundSize:'100% 100%'}"></span>
              <span class="fontSize2p">{{item.discussNum}}</span>
              <span id="chatIcon" :style="{background:'url('+bgIcon1+') center center no-repeat',backgroundSize:'100% 100%'}"></span>
		        </p>    	
		    </div>
  		</li>
  	</ul>
  	<div id="footerHolder"></div>
  </div>

</template>
<script>
  import store from './../../store/index.js'
  export default {
    name: 'my',
    data () {
      return {
        bgIcon1: require('./../../assets/GeneralIcon/Chat_yes.png'),
        bgIcon2: require('./../../assets/GeneralIcon/thumbs.png'),
        items: [
        ]
      }
    },
    methods: {
      discussInfo (params) {
        this.$router.push({name: 'discussInfo', params: {info: params}})
      }
    },
    computed: {
      newItems () {
        let temp = []
        for (let i = 0; i < this.items.length; i++) {
          if (parseInt(this.items[i]['discussNum']) !== 0) {
            temp.push(this.items[i])
          }
        }
        return temp.sort(compare('discussNum'))
      }
    },
    created: function () {
      let publicActiveObj = store.state.publicActiveInfo
      let userObj = store.state.userInfo
      for (var key in publicActiveObj) {
        let arr = publicActiveObj[key]
        for (let i = 0; i < arr.length; i++) {
          let tempObj = {}
          tempObj['imgSrc'] = userObj[key].logoImgSrc
          tempObj['userName'] = userObj[key].userName
          tempObj['descTime'] = arr[i].time
          tempObj['yiti'] = arr[i].theme
          tempObj['detail'] = arr[i].detail
          tempObj['discussNum'] = arr[i].discussNum
          tempObj['goodNum'] = arr[i].goodNums
          tempObj['discussInfoId'] = arr[i].discussInfoId
          tempObj['publicActiveInfoId'] = arr[i].publicActiveInfoId
          tempObj['userInfoID'] = key
          tempObj['isThumbs'] = arr[i].isThumbs
          this.items.push(tempObj)
        }
      }
      // console.log(this.items)
    },
    store
  }
  // 排序方法
  function compare (property) {
    return function (a, b) {
      var value1 = parseInt(a[property])
      var value2 = parseInt(b[property])
      return value2 - value1
    }
  }

</script>
<style>
  #box{
  	text-align: center;
  	font-size: 40px;
  }
  #box ul li{
	height: 100%;
	width: 100%;
	margin: 0px;
	overflow: hidden;
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	border-bottom: 4px solid #EEEEEE;
  }
  #box ul li:nth-child(2n+1){
	/*background-color: rgba(0,255,255,.05)*/
  }
  #pgA-area1{
	float: left;
	width:200px;
  }
	.imgSize150{
		margin-top: 1rem;
		width:7.5rem;
		height:7.5rem;
		border-radius: 50%;
	}
  #pgA-area2{
	width:98%;
	text-align: left;
  }
  #pgA-area2 .one{
 	position: relative;
	padding-top: 1rem;
}
 #pgA-area2 .two{
 	position: relative;
	padding-top: 1rem;
  padding-right: 1.5rem;
  font-size: 30px;
}
 #pgA-area2 .three{
 	position: relative;
	padding: 1rem 2rem;
  overflow: hidden;
}
 #pgA-area2 span.right0{
  display:inline-block;
  text-align: right;
  position: absolute;
  right: 30px;
  height: 57px;
  line-height:57px;
  color: rgba(0,0,0,.5);
}
.fontSize2p7{
	font-size: 36px;
}
.fontSize2p{
	font-size: 32px;
}
 #pgA-area2 .three span{
    display: block;
    float: right;
    position: relative;
    height: 57px;
    line-height:57px;
 }

  #chatIcon{
  display: inline-block;
  width: 60px;
  height: 57px;
  line-height:57px;
  /*background: url("./../../assets/GeneralIcon/Chat_yes.png") -10px -10px no-repeat;*/
  background-size: 4.5rem;
 }
  #thumbsIcon{
  display: inline-block;
  width: 60px;
  height: 57px;
  /*background: url("./../../assets/GeneralIcon/thumbs.png") -10px -10px no-repeat;*/
  background-size: 4.5rem;
  margin-left:2rem;
 }
  #footerHolder{
  	height: 160px;
  }
</style>

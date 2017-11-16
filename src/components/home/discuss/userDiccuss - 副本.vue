<template>
  <div class="userDiscussion">
    <!--主要内容-->
        <section class="user_discussion_list">
          <ul class="user_discussion_list_ul">
            <li class="user_discussion_list_li" v-for="(item,index) in currentDiscussion.userDiscussionList" :key="index">
              <section class="problemDescription_header">
                <div class="problemDescription">
                  <div class="problemDescription_top">
                    <div class="problemDescription_name">
                      <p>{{item.userName}}</p>
                    </div>
                    <div class="problemDescription_timer">
                      <p>{{item.timer}}</p>
                    </div>
                  </div>
                  <div class="problemDescription_user_content" v-if="!item.isInputContent"> <p>{{item.content}}</p></div>
                  <div class="problemDescription_user_content"  v-if="item.source && item.isInputContent">
                     <textarea class="editor_textarea" v-model="item.content" @keyup.enter="inputContent('editored', index)"></textarea>
                  </div>
                </div>
                <div class="inputComment" v-if="!item.source">
                  <textarea v-model="message" class="input_text" style="overflow: hidden" placeholder="写下你的想法" @keyup.enter="submit(index)"></textarea>
                </div>
                <div class="userDiscussionContent_editor"  v-if="item.source">
                  <p class="userDiscussionContent_editor_p" @click="inputContent('editor', index)">编辑</p>
                </div>
                <div>
                </div>
              </section>
               <section class="discuss_list">
                <div class="discuss_list_top">
                  <div class="Sort_describe"><p>高票回答</p></div>
                  <div class="line"></div>
                </div>
                <ul class="discuss_list_ul" :class="{fixationHight: item.isFixationHight && item.discussionList.length > 2}">
                  <!--<li v-for="(item1,index,) in item.discussionList" :key="index" class="discuss_list_li">-->
                  <li v-for="(item1,index) in showMoreResponse(item)" :key="index" class="discuss_list_li">
                    <div class="item_left">
                      <p :style="{backgroundImage: 'url('+ item1.headerImg +')'}"></p>
                    </div>
                    <div class="item_mian">
                      <span>{{item1.name}}</span>
                      <span class="participants">{{item1.discuss}}</span>
                    </div>
                    <div class="item_right">
                      <span class="participants">{{item1.time}}</span>
                      <p class="participants_img_box" @click="thumbComments(index,item1,item)">
                        <span class="participants_img" :style="{backgroundImage: 'url('+ item1.likeImg +')'}"></span>
                        <span class="participants_likeNumber">{{item1.likeNumber}}</span>
                      </p>
                    </div>
                  </li>
                  <div class="lookMore" v-if="item.discussionList.length > 2" :class="{active: !item.isFixationHight}"><p @click="lookMore(index)">{{lookMoreText}}</p></div>
                </ul>
              </section>
            </li>
          </ul>
        </section>
        <section class=""></section>
        <section class=""></section>
      <alert-tip v-if="showAlert" :showHide="showAlert" @closeTip="closeTip" :alertText="alertText"></alert-tip>
    <!--我要提问按钮-->
    <div class="bottom_btn" @click="addPath('/question')">
      <div class="bottom_btn_box">
        <p class="bottom_btn_img"></p>
        <p>提问</p>
      </div>
    </div>
  </div>
</template>
<script>
  import headTop from '../../communal/header/head'
  import alertTip from '../../communal/alertTip'
  import {mapState, mapMutations} from 'vuex'
  export default {
    data () {
      return {
        headTitle: '探议题',
        alertText: '',
        showAlert: false,
        outArryIndex: '',
        showUserDiscussion: false,
        inItemeArry: [],
        message: null,
        lookMoreText: '查看更多...'
      }
    },
    methods: {
      ...mapMutations(['SWITHER_TOPIC', 'SUBMIT_COMMENT', 'THUMB_COMMMENTS', 'CLICK_EDITOR', 'LOOK_MORE_COMMENTS']),
      addPath (path) {
        this.$router.push(path)
      },
      switchIssues (index) {
        if (index === 2) {
          if (this.currentBook.discussionMeun.length < 3) {
            this.alertText = '你还没有议题'
            this.showAlert = true
            return
          } else {
            this.showUserDiscussion = true
            this.SWITHER_TOPIC({index: index})
          }
        } else {
          this.showUserDiscussion = false
          this.SWITHER_TOPIC({index: index})
        }
      },
      closeTip () {
        this.showAlert = false
      },
      submit (index) {
        var now = new Date()
        var year = now.getFullYear()       // 年
        var month = now.getMonth() + 1    // 月
        var day = now.getDate()          // 日
        var timer = year + '/' + month + '/' + day
        var obj = {
          headerImg: require('../../../images/乐创编/JINSHANSHAN.png'),
          name: '我',
          time: timer,
          likeNumber: 0,
          isLiked: false,
          likeImg: require('../../../images/icon/thumbs.png'),
          discuss: this.message
        }
  //       判断输入内容不能为空
        if (this.message.length > 1) {
  //          console.log(this.message.length)
          this.SUBMIT_COMMENT({obj: obj, tpye: 'user', index: index})
        }
        this.message = null
      },
  //    点赞
      thumbComments (index, item, out) {
        console.log(out)
        for (var i = 0; i < this.currentDiscussion.userDiscussionList.length; i++) {
          if (this.currentDiscussion.userDiscussionList[i] === out) {
            this.outArryIndex = i
            console.log(this.outArryIndex)
            if (item.isLiked === false) {
              this.THUMB_COMMMENTS({index: index, tpye: 'user', outIndex: this.outArryIndex})
            }
          }
        }
      },
//    编辑
      inputContent (type, index) {
        this.CLICK_EDITOR({index: index, type: type})
      },
      filterArry (vule, itme) {
        this.inItemeArry = vule
        return vule
      },
      lookMore (index) {
        console.log(index)
        this.LOOK_MORE_COMMENTS({index: index})
      },
      showMoreResponse (item) {
        if (item.isFixationHight === true) {
          this.lookMoreText = '查看更多...'
          return item.discussionList.slice(0, 2)
        } else {
          this.lookMoreText = '收起更多...'
          return item.discussionList
        }
      }
    },
    components: {
      headTop,
      alertTip
    },
    computed: {
      ...mapState(['currentBook', 'currentDiscussion', 'swichList']),
      //    下拉和隐藏
      isFixationHight: function (lenght) {
        if (lenght > 2) {
          return {
            fixationHight: true
          }
        } else {
          return {
            fixationHight: false
          }
        }
      }
    }
  }
</script>
<style lang="scss" scoped>
  @import "../../../style/mixin.scss";
.userDiscussion{
  margin: 0;
  padding-top: 7.3rem;
}
  .problemDescription{
    padding: 2rem 0rem;
    letter-spacing: 0.6rem ;
  }
  .problemDescription_header{
    padding: 0 1rem;
    background: #ffffff;
  }
  .problemDescription p{
    font-size: 2.25rem;
  }
  .problemDescription_timer{
    display: flex;
    font-size: 2rem;
    color: #707070;
    letter-spacing:0rem ;
    justify-content: flex-end;
  }
  .editor_textarea{
    width: 100%;
    height: 10rem;
    font-size: 2.2rem;
  }
  .fixationHight{
    /*height: 10rem;*/
  }
  .selectorButton span{
    margin-left: 2rem;
    padding: 0.5rem 2rem;
    border-right: solid 1px #000000;
  }
  .selectorButton span:last-child{
    border-right:0;
  }
  .selectorButton span.active{
    color: #1a9fff;
  }
  .inputComment{
    width: 100%;
    display: flex;
    justify-content: center;
  }
  .input_text{
    height: 10rem;
    width: 96%;
    margin-bottom: 2rem;
    font-size: 3rem;
  }
  /*list部分*/
  .discuss_list{
    margin-top: 4rem;
  }
  .Sort_describe{
    margin-bottom: 1rem;
  }
  .line{
    height: 1px;
    background: #000;
  }
  .item_left{
    margin: 2rem;
    display: flex;
  }
  .item_left p{
    width: 7.5rem;
    height: 7.5rem;
    margin: auto;
    background: no-repeat;
    background-size:100% 100%;
  }
  .discuss_list_li{
    display: flex;
    justify-content: space-between;
    background: #ffffff;
    padding: 1rem 0;
    border-bottom: solid 1px #000000;
  }
  .item_mian, .item_right{
    display: flex;
    flex-direction: column;
    justify-content: space-around;
  }
  .item_mian{
    flex: 1;
  }
  .item_right{
    text-align: center;
  }
  .participants_img_box{
    display: flex;
    justify-content: flex-end;
  }
  .participants_img{
    width: 5rem;
    height: 5rem;
    background-size:100% 100%;
  }
  .participants_likeNumber{
    line-height: 5rem;
  }
  .bottom_btn{
    position: fixed;
    width: 10rem;
    height: 10rem;
    z-index: 100;
    right: 0;
    bottom: 4rem;
    font-size: 4rem;
    display: flex;
    padding: 2rem;
  }
  .bottom_btn_box{
    width: 100%;
    height: 100%;
    background-color: #1a9fff;
    border-radius: 50%;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .bottom_btn_box p{
    line-height: 4rem;
    font-size: 3rem;
    width: 100% ;
    color: #ffffff;
    text-align: center;
  }
  .bottom_btn_box p:first-child{
    height: 50%;
    width: 100%;
    position: relative;
    background: url("../../../images/icon/Write_white.png") no-repeat center;
  }
  .bottom_btn_box p:last-child{
    position: relative;
    bottom: 1.1rem;
  }
  .discuss_submit p{
    text-align: center;
  }
  /* 用户议题 */
  .problemDescription_top{
    padding: 2rem 0;
    color: #707070;
    display: flex;
    justify-content: space-between;
  }
  .problemDescription_user_content p{
    overflow: hidden;
    font-size: 1.33rem;
    /*font-size: 2.5rem;*/
  }
 .problemDescription_top .problemDescription_name p{
   font-size: 2.5rem;
  }
  .userDiscussionContent_editor{
    color: #1a9fff;
    display: flex;
    justify-content: flex-end;
  }
  .userDiscussionContent_editor_p::before{
    content:"";
    margin-right: 0.5rem;
    padding-top: 0.5rem;
    padding-right: 2rem;
    background: url("../../../images/icon/Writet_点击.png")no-repeat center;
  }
  .user_discussion_list_li{
    background-color: #fff;
    margin-bottom: 1rem;
  }
  .lookMore{
    display: flex;
    justify-content: center;
  }
  .lookMore p{
    color: #1a9fff;
    line-height: 4rem;
  }
  .lookMore p::before{
    content:"";
    margin-right: 0.5rem;
    padding-right: 2rem;
    background: url("../../../images/icon/down.png")no-repeat center;
  }
  .lookMore.active p::before{
    background: url("../../../images/icon/up.png")no-repeat center;
  }
</style>

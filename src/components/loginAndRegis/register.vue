<template>
  <div class="loginAndRegist">
    <!--顶部导航-->
    <head-top go-back='true'></head-top>
    <section class="content">
      <section class="top">
        <div class="top_logo">
          <div class="top_img" >
            <p>LOGO</p>
          </div>
        </div>
      </section>
      <section class="loginForm">
        <div class="input_container phone_number" >
          <input type="text" placeholder="请填写手机号码" name="phone" maxlength="11" v-model="phoneNumber">
        </div>
        <div class="input_container mobileCode" >
          <div class="passWord_box">
            <input type="text" placeholder="请输入验证码" name="mobileCode" maxlength="6" v-model="mobileCode">
            <span class="verification" @click="">获取验证码</span>
          </div>
        </div>
        <div class="input_container password" >
          <div class="passWord_box">
            <input v-if="!showPassword" type="password" placeholder="请设置你的密码（不少于6位）"  v-model="passWord">
            <input v-else type="text" placeholder="请输入密码"  v-model="passWord">
          </div>
        </div>
        <div class="loge_btn" @click="mobileLogin">
          <div class="loge"> <p>注册</p></div>
        </div>
      </section>
      <section class="bottom">
        <div><p>有问题？请点这里</p></div>
      </section>
      <alert-tip v-if="showAlert" :showHide="showAlert" @closeTip="closeTip" :alertText="alertText"></alert-tip>
    </section>
  </div>
</template>
<script>
  import headTop from '../communal/header/head'
  import alertTip from '../communal/alertTip'
  import {mapState, mapMutations} from 'vuex'
  export default {
    name: 'loginAndRegist',
    data () {
      return {
        passWord: null,
        phoneNumber: null,
        mobileCode: null, // 短信验证码
        showPassword: false,
        showAlert: false,
        alertText: null
      }
    },
    methods: {
      ...mapMutations(['increment', 'SAVE_USERINFO']),
      addPath (path) {
        this.$router.push(path)
      },
      changePassWordType () {
        this.showPassword = !this.showPassword
      },
      mobileLogin () {
        if (!this.rightPhoneNumber) {
          this.showAlert = true
          this.alertText = '请输入正确的手机号'
          return
        } else if (!this.rightPassWord) {
          this.showAlert = true
          this.alertText = '请输入密码'
          return
        } else if (!this.mobileCode) {
          this.showAlert = true
          this.alertText = '验证码不正确'
          return
        } else {
          this.SAVE_USERINFO({ userInfo: {
            phoneNumber: this.phoneNumber,
            passWord: this.passWord
          }})
          console.log(this.userInfo)
          this.addPath('/articles')
        }
      },
      closeTip () {
        this.showAlert = false
      }
    },
    components: {
      headTop,
      alertTip
    },
    computed: {
      ...mapState(['userInfo']),
      // 判断手机号码
      rightPhoneNumber: function () {
        return /^1\d{10}$/gi.test(this.phoneNumber)
      },
      // 判断手机号码
      rightPassWord: function () {
        return /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,16}$/gi.test(this.passWord)
      }
    }
  }
</script>
<style lang="scss" scoped>
  @import "../../style/mixin.scss";
 html, body{
    height: 100%;
    display: flex;
   flex-direction: column;
   justify-content: center;
  }
  .content{
  @include allcover();
    width: 100%;
    height: 78rem;
    font-size: 2.5rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: 0;
    padding: 0;
    margin-top: 8rem;
  }
  .top_logo{
    margin-bottom: 3rem;
    display: flex;
    justify-content: center;
  }
  .top_img{
    height: 14rem;
    width: 14rem;
    border-radius: 50%;
    border: solid 2px #7f7f7f;
    display: flex;
    justify-content: center;
  }
  .top_img p{
    line-height: 14rem;
  }
  .loginForm{
    margin: 0 2.67rem;
    display: flex;
    flex-direction: column;
  }
  .input_container{
    display: flex;
    margin-bottom: 1rem;
  }
  .input_container input{
    width: 100%;
    font-size: 3rem;
    line-height: 6rem;
    border: solid 2px #7f7f7f;
  }
  .passWord_box{
    width: 100%;
    position: relative;
  }
  .passWord_box .verification{
    position: absolute;
    width: 25rem;
    text-align: center;
    line-height: 6rem;
    height: 100%;
    border-left: solid 1px #000000;
    right: 0;
    bottom: 0;
  }
  .loge_btn{
    margin-top: 1rem;
  }
  .loge_btn .loge{
    background-color: #1a9fff;
    font-size: 4rem;
    width: 100%;  display: flex;
    justify-content: center;
    border-radius: 0.5rem;
  }
  .loge_btn p{
    margin-top: 1rem;
    margin: 0 ;
    line-height: 8rem;
  }
  .bottom{
    margin: 0 2rem;
    margin-top: 10rem;
  }
  .bottom p{
    text-decoration: underline;
  }
</style>

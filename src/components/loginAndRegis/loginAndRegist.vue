<template>
  <div class="loginAndRegist">
    <section class="top">
        <div class="top_logo">
          <div class="top_img" >
            <p>LOGO</p>
          </div>
        </div>
    </section>
    <section class="but_list">
      <div class="top_text" >
        <p>登录后才能享有更多功能喔！</p>
      </div>
      <div class="btn">
        <div class="regist_btn" @click="addPath('/register')"><p>注册</p></div>
        <div class="loge_btn" @click="addPath('/login')"><p>登录</p></div>
        <div class="look_around" @click="addPath('/articles')"><p>先去看看</p></div>
      </div>
    </section>
  </div>
</template>
<script>
  export default {
    name: 'loginAndRegist',
    data () {
      return {
        userInfo: false
      }
    },
    methods: {
      addPath (path) {
        this.$router.push(path)
      }
    }
  }
</script>
<style lang="scss" scoped>
  @import "../../style/mixin.scss";
  .loginAndRegist{
  @include allcover();
    width: 100%;
    display: flex;
    height: 78rem;
    flex-direction: column;
    justify-content: center;
    font-size: 4rem;
    margin-top: 8rem;
  }
  .top_logo{
    display: flex;
    justify-content: center;
  }
  .top_img{
    height: 20rem;
    width: 20rem;
    border-radius: 50%;
    border: solid 2px #7f7f7f;
    display: flex;
    justify-content: center;
  }
  .top_img p{
    line-height: 20rem;
  }
  .top_text p{
    font-size:3rem;
    margin: 4rem 0 4rem;
  }
  .but_list{
    margin: 0rem 5rem;
  }
  .btn div{
    background-color: #1a9fff;
    text-align: center;
    color: #ffffff;
    margin-bottom: 3rem;
    border-radius: 0.5rem;
  }
  .btn div:last-child p{
    background-color: #ffffff;
    color: #000000;
    border: solid 1px #7f7f7f;
    border-radius: 0.5rem;
  }
  .btn div p{
    line-height: 8rem;
  }
</style>

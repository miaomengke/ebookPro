<template>
  <div>
  	<!--顶部导航-->
    <head-top  go-back='true' :path="'/myInfo'" title='true'  headTitle='修改个人信息' message =''></head-top>
    <div class="content">
      <div class="titleC">
        <img :src="personInfo.logoImgSrc" class="imgSize200" alt="logo">  
        <p v-text="'ID: '+this.personInfo.ID"></p>
      </div>
      <div class="form">
        <p><label>姓名：
          <label @click="modifyName()" v-text="personInfo.userName"></label>
        </label></p>
        <p>
          <label for="sex">性别：</label>
          <input type="radio" name="sex" value="male" checked>男
          <input type="radio" name="sex" value="female">女
          <input type="radio" name="sex" value="no">保密
        </p>
        <p>
          <label>个性签名：</label>
          <span @click="modifySignature()" v-text="personInfo.signature"></span>
        </p>
        <p>
          <label>学校：</label>
          <label v-text="schoolInfo" @click="modifySchoolInfo()"></label>
        </p>
        <p>
          <label>班级：</label>
          <label @click="modifyClassInfo()" v-text="classInfo"></label>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
  import headTop from '../../communal/header/head'
  import footGuide from '../../communal/footer/foot'
  import store from '@/store/index'
  export default {
    name: 'modify',
    data () {
      return {
        sonMsg: '',
        personInfo: store.state.userInfo['24454565']
      }
    },
    components: {
      headTop,
      footGuide
    },
    methods: {
      modifySchoolInfo () {
        this.$router.push({name: 'modifyInfo', params: {schoolInfo: store.state.schoolInfo[this.personInfo.schoolInfoId]}})
        console.log(this.userInfo)
      },
      modifySignature () {
        this.$router.push({name: 'modifyInfo', params: {signature: this.personInfo.signature}})
      },
      modifyName () {
        this.$router.push({name: 'modifyInfo', params: {userName: this.personInfo.userName}})
      },
      modifyClassInfo () {
        this.$router.push({name: 'modifyInfo', params: {classInfo: store.state.classInfo[this.personInfo.classInfoId]}})
      }
    },
    computed: {
      schoolInfo: function () {
        return store.state.schoolInfo[this.personInfo.schoolInfoId]
      },
      classInfo: function () {
        return store.state.classInfo[this.personInfo.classInfoId]
      }
    },
    mounted: function () {
      if (this.$route.params.signature) {
        this.personInfo.signature = this.$route.params.signature
      } else if (this.$route.params.userName) {
        this.personInfo.userName = this.$route.params.userName
      }
      // console.log(document.getElementsByTagName('input')[parseInt(this.personInfo.sex) - 1])
      document.getElementsByTagName('input')[parseInt(this.personInfo.sex) - 1].setAttribute('checked', '')
    },
    store
  }
</script>

<style>
.content{
    padding:30px ;
    font-size:36px;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
    text-align: center;
}
 .titleC{
  padding-top: 100px;
  padding-bottom: 40px; 
  border-bottom: 1px solid black;
 }
 .titleC p{
  color: #999999;
 }
 .form{
  margin: 0 auto;
  padding-top: 50px;
  text-align: left;
 }
 .form p{
  padding-bottom: 40px;
 }
 .form input{
  width: 35px;
  height: 35px;
 }
</style>

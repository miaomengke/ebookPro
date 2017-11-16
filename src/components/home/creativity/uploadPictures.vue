<template>
  <div class="creativity">
    <!--顶部导航-->
    <head-top go-back='true' title='true'  :head-title="headTitle"></head-top>
    <section class="main" v-if="isShow">
      <section class="item">
        <!--<div class="item_box">-->
          <input id="input_camera" type="file" accept="image/*" capture="camera" @change="onFileChange">
          <!--<input id="input_camera" type="file" accept="video/*" capture="camcorder" >-->
          <!--<input id="input_camera" type="file" accept="audio/*" capture="microphone" >-->
          <label for="input_camera" class="input_lable">
            <div class="item_left"><p class="camera_img" :style="{backgroundImage: 'url('+ cameraImg +')'}"></p></div>
            <div class="item_right"><p>拍照上传</p></div>
          </label>
        <!--</div>-->
      </section>
      <section class="item">
        <input id="input_img" type="file" class="form-control" @change="onFileChange" accept="image/*">
        <label for="input_img" class="input_lable">
          <div class="item_left"><p class="photo_img" :style="{backgroundImage: 'url('+ photoImg +')'}"></p></div>
          <div class="item_right"><p>选择照片</p></div>
        </label>
      </section>
    </section>
    <complete v-if="!isShow"></complete>
  </div>
</template>
<script>
  import headTop from '../../communal/header/head'
  import complete from '../creativity/uploadImgcomplete'
  import {mapMutations} from 'vuex'
  export default {
    data () {
      return {
        headTitle: '上传图片',
        isShow: true,
        cameraImg: require('../../../images/icon/Camera.png'),
        photoImg: require('../../../images/icon/Image.png')
      }
    },
    methods: {
      ...mapMutations(['CHOOSE_IMGE']),
      addPath (path) {
        this.$router.push(path)
      },
      onFileChange (e) {
//        console.log('112222222222222')
        this.isShow = false
        var files = e.target.files || e.dataTransfer.files
        if (!files.length) {
          return
        }
        this.createImage(files)
      },
      createImage (file) {
        if (typeof FileReader === 'undefined') {
          alert('您的浏览器不支持图片上传，请升级您的浏览器')
          return false
        }
        var vm = this
        var leng = file.length
        for (var i = 0; i < leng; i++) {
          var reader = new FileReader()
          reader.readAsDataURL(file[i])
          reader.onloadend = function (e) {
            vm.CHOOSE_IMGE({chooseImageUrl: e.target.result})
          }
        }
      },
      delImage (index) {
        this.images.shift(index)
      },
//    清空选择的图片
      removeImage (e) {
        this.CHOOSE_IMGE({chooseImageUrl: ''})
      }
    },
    components: {
      headTop,
      complete
    },
    computed: {
    },
    created: function () {
    }
  }
</script>
<style lang="scss" scoped>
  @import "../../../style/mixin.scss";
  .main{
    position: fixed;
    top:10.3rem;
    left: 0;
    right: 0;
    bottom: 0;
    color: #ffffff;
    font-size: 3rem;
  }
  .main .item{
    margin: 5rem 2.5rem;
    height: 10rem;
    background-color: #1a9fff;
    display: flex;
    justify-content: center;
    border-radius: 0.5rem;
  }
  .item_box{
    display: flex;
    justify-content: center;
  }
  .item_left p{
    width: 10rem;
    height: 10rem;
  }
  .camera_img{
    background:no-repeat center;
  }
  .photo_img{
    background:no-repeat center;
  }
  .item_right{
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .item_right p{
    text-align: center;
  }
  input {
    position: absolute;
    left: -9999px;
  }
  .input_lable{
    width: 100%;
    display: flex;
    justify-content: center;
  }
</style>

<script>
import Cropper from 'cropperjs'
import 'cropperjs/dist/cropper.css'

export default {
  name: 'ImageCropper',
  props: {
    src: String,
    callback: Function
  },
  data(){
    return {
      cropper: null,
      isCropping:false
    };
  },
  methods: {
    selectImgFile(event){
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => {
        //获取base64编码的图片数据
        const imageData = reader.result;
        if(imageData == null) 
          return;
        this.$refs.cropImg.src = imageData;
        this.$refs.cropImg.style.display = 'block';
        this.isCropping = true;
        this.cropImage();
      };
      
    },
    cropImage() {
      this.$data.cropper = new Cropper(this.$refs.cropImg,{
        aspectRatio:1 // 裁剪比例
      });
    },
    saveImage(){
      var canvas = this.cropper.getCroppedCanvas();
      var dataURL = canvas.toDataURL();
      this.$refs.image.src = dataURL;
      this.$data.cropper.destroy();
      this.isCropping = false;
      if(this.$props.callback != null) 
        this.callback(dataURL);
    }
  }
}
</script>

<template>
<div>
  <div style="display: flex;align-items:end;">
    <img class="image" ref="image" :src="src"/>

    <div class="button primary" style="margin: 0 10px;">
      <b><label for="file-input">选择图片</label></b>
    </div>
    <button class="primary" ref="cropBtn" @click="saveImage" v-show="isCropping"><b>裁剪</b></button>

    <input id="file-input" type="file" @change="selectImgFile" style="width:2px;position:absolute;opacity:0;"/>
  </div>
  
  <img :src="src" alt="图片" class="cropImg" ref="cropImg" v-show="isCropping"/>

</div>
</template>

<style>

.image {
  height:80px;
  width:auto;
}

.cropImg {
  display: block;
  max-width: 70vw;
  margin: 5px 2px;
  box-sizing: border-box;
}
</style>
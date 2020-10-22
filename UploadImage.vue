<template>
  <div class="upload-images">
    <input
      type="file"
      accept="image/*"
      multiple="multiple"
      @change="handleImgSelected"
    />
    <div class="upload-images-demo" v-if="imgSrcs.length">
      <img
        class="upload-images-content"
        :key="index"
        v-for="(img, index) in imgSrcs"
        :src="img"
        alt=""
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imgSrcs: []
    };
  },
  methods: {
    handleImgSelected () {
        this.imgSrcs = []
        let finish = this.getArrayBuffer()
        finish.then((arrayBuffers)=>{
            // return console.log(this.imgSrcs) //從這裡接 API
        })
    },
    async getArrayBuffer() {
      let files = event.target.files
      let arrayBuffers = []
      for (let index = 0; index < files.length; index++) {
          let arrayBuffer = await this.handleShowImgs(files[index]);
          arrayBuffers.push(arrayBuffer)
      }
      return arrayBuffers
    },
    handleShowImgs(imgSource) {
      return new Promise((resolve, reject) => {
        const _self = this;
        let curFile = imgSource; // 透過 input 取得的 file object
        let reader = new FileReader();
        reader.onload = function(file) {
          resolve(file.target.result);
        };
        reader.readAsDataURL(curFile);
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.upload-images {
  .upload-images-demo {
    .upload-images-content {
      margin: 10px 10px 0 0;
      width: 100px;
      height: 100px;
      display: inline-block;
    }
  }
}
</style>
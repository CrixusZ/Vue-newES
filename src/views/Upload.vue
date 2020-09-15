<template>
  <div>
    <!-- multiple 多选 -->
    <label for="upload" class="choose-btn">选择图片上传</label>
    <input
      type="file"
      multiple id="upload"
      accept="image/*"
      @change="onChange"
      ref="file"
      style="display: none">
    <p class="tip">
      提示：一次可以选择多张图片，最多不超过9张，大小不超过1m
    </p>
  </div>
</template>

<script>
export default {
  methods: {
    onChange () {
      // 可选链判断
      const newFlies = this.$refs?.file?.files
      // 校验
      if (newFlies.length > 9) {
        console.log('最多可以一次选择9张')
        return false
      }
      const files = []
      for (const file of newFlies) {
        const size = file.size / 1024 / 1024 // 把单位转化为m
        if (size > 1) {
          console.log('请选择1M以内的图片')
          return false
        }
        files.push(file)
      }
      this.uploadFilesByOss(files)
    },
    // 上传多图片到阿里云
    uploadFilesByOss (files) {
      console.log(files)
    }
  }
}
</script>

<style scoped>
.choose-btn{
  display: block;
  margin: 0 auto;
  width: 150px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  background-color: #42b983;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
}
.tip{
  color: #ccc;
}
</style>

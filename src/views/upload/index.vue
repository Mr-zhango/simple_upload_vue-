<template>
  <div>
    <div class="upload">
      <h3>请上传原始数据文件</h3>
      <el-upload
        ref="upload"
        class="upload-demo"
        :action="uploadUrl"
        :show-file-list="false"
        :auto-upload="false"
        :on-success="handleSuccess"
        :on-progress="uploadProcess"
        :on-change="handleChange"
      >
        <el-button
          slot="trigger"
          type="primary"
          class="video-btn"
          size="small"
        >
          选取文件
        </el-button>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传文件<i class="el-icon-upload el-icon--right" /></el-button>
      </el-upload>
      <div v-if="fileName" class="file"><i class="el-icon-document" /> {{ fileName }}</div>
      <el-progress
        v-if="isPercentage"
        class="progress"
        :stroke-width="10"
        :percentage="percentage"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      uploadUrl: 'http://localhost:8085/file/saveFile',
      isPercentage: false, // 是否显示进度条
      percentage: 0,
      fileName: ''
    }
  },
  mounted() {},
  methods: {
    // 上传按钮
    submitUpload() {
      this.$refs.upload.submit()
    },

    // 上传列表
    handleChange(file, fileList) {
      this.fileName = file.name
    },

    // 进度条
    uploadProcess(event, file, fileList) {
      console.log(file.percentage)
      this.isPercentage = true
      this.percentage = file.percentage.toFixed(0) * 1
    },

    // 上传成功回调
    handleSuccess(res, file) {
      this.isPercentage = true
      this.percentage = file.percentage.toFixed(0) * 1
      if (res.code === '0000') {
        this.$message('上传成功!')
      } else {
        this.$message.error('上传失败，请重新上传！')
      }
    }
  }
}
</script>

<style scoped lang="scss">
.upload {
  width: 350px;
}

.progress {
  margin-top: 20px;
}

.file {
  color: #606266;
  font-size: 14px;
  margin-top: 10px;
}
</style>

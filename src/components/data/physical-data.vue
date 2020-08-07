<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="120px">
      <el-form-item label="熔化温度范围">
        <el-upload
          class="upload-demo"
          drag
          :action="image"
          multiple
          :limit="1"
          name="upload_file"
          :on-success="handlePictureCardPreview"
          :on-exceed="handErr"
        >
          <i class="el-icon-upload"></i>
          <div class="el-upload__text">
            将文件拖到此处，或
            <em>点击上传</em>
          </div>
          <div class="el-upload__tip" slot="tip" style="color:red">只能上传一张图片 jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>
      <el-form-item label="相 变 点">
        <el-upload
          class="upload-demo"
          drag
          :action="image"
          multiple
          :limit="1"
          name="upload_file"
          :on-success="handlePicture"
          :on-exceed="handErr"
        >
          <i class="el-icon-upload"></i>
          <div class="el-upload__text">
            将文件拖到此处，或
            <em>点击上传</em>
          </div>
          <div class="el-upload__tip" slot="tip" style="color:red">只能上传一张图片 jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>
      <el-form-item label="密 度" prop="density">
        <el-input v-model="dataForm.density" placeholder="密 度" type="textarea"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "physicalData",
  data() {
    return {
      image: "",
      dataForm: {
        categoryId: "",
        meltingTemperature: "", //熔化温度范围
        image: "",
        phaseTransition: "", //相变点
        density:""
      },
      inputVisible: false,
    };
  },
  components: {},
  computed: {},
  beforeMount() {},
  mounted() {},
  methods: {
    init(data, id) {
      if (data) {
        this.dataForm = data;
      } else {
        this.dataForm.categoryId = id;
        this.dataForm.meltingTemperature = "";
        this.dataForm.phaseTransition = "";
        this.dataForm.density = "";
      }
      this.image = this.$http.adornUrl(
        `/sys/filemanager/uploadimg?token=${this.$cookie.get("token")}`
      );
    },
    handlePictureCardPreview(response, file, fileList) {
      console.log(response)
      this.dataForm.meltingTemperature = response.filename;
    },
    handlePicture(response, file, fileList) {
      console.log(response)

      this.dataForm.phaseTransition = response.filename;
    },
    handErr() {
      this.$message.error("请删除已有已有文件");
    },
  },
  watch: {},
};
</script>
<style lang="scss" scoped>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}

.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}

.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>


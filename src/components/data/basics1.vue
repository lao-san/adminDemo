<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="120px" :rules="dataRule">
      <el-form-item label="标题图" prop="image">
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

      <el-form-item label="材料牌号" prop="code">
        <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
      </el-form-item>

      <el-form-item label="关键词" prop="keyword">
        <el-tag
          :key="tag"
          v-for="tag in dynamicTags"
          closable
          :disable-transitions="false"
          @close="handleClose(tag)"
        >{{tag}}</el-tag>
        <el-input
          class="input-new-tag"
          v-if="inputVisible"
          v-model="inputValue"
          ref="saveTagInput"
          size="small"
          @keyup.enter.native="handleInputConfirm"
          @blur="handleInputConfirm"
        ></el-input>
        <el-button v-else class="button-new-tag" size="small" @click="showInput">添加新关键词</el-button>
      </el-form-item>

      <el-form-item label="作者" prop="author">
        <el-input v-model="dataForm.author" placeholder="作者"></el-input>
      </el-form-item>

      <el-form-item label="来源" prop="source">
        <el-input v-model="dataForm.source" placeholder="来源" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="参考文献" prop="reference">
        <el-input v-model="dataForm.reference" placeholder="参考文献" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="材料概述" prop="overview">
        <el-input v-model="dataForm.overview" placeholder="材料概述" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="材料标准" prop="standard">
        <el-input v-model="dataForm.standard" placeholder="材料标准" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="熔炼工艺" prop="meltingProcess">
        <el-input v-model="dataForm.meltingProcess" placeholder="熔炼工艺" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="化学成分" prop="chemicalComposition">
        <el-input v-model="dataForm.chemicalComposition" placeholder="化学成分" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="热处理制度" prop="heatTreatment">
        <el-input v-model="dataForm.heatTreatment" placeholder="热处理制度" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="主要规格" prop="mainSpecifications">
        <el-input v-model="dataForm.mainSpecifications" placeholder="主要规格" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="供应状态" prop="supplyStatus">
        <el-input v-model="dataForm.supplyStatus" placeholder="供应状态" type="textarea"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "basics",
  data() {
    return {
      image: "",
      dataForm: {
        categoryId: "",
        supplyStatus: "",
        mainSpecifications: "",
        heatTreatment: "",
        chemicalComposition: "",
        meltingProcess: "",
        standard: "",
        overview: "",
        reference: "",
        standard: "",
        source: "",
        keyword: "",
        code: "",
        author: "",
        image: "",
      },
      dataRule: {
        code: [
          { required: true, message: "材料牌号不能为空", trigger: "blur" },
        ],
        categoryId: [
          { required: true, message: "材料类型id不能为空", trigger: "blur" },
        ],
      },
      dynamicTags: [],
      inputVisible: false,
      inputValue: "",
    };
  },
  components: {},
  computed: {},
  beforeMount() {},
  mounted() {},
  methods: {
    handleClose(tag) {
      this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1);
    },
    showInput() {
      this.inputVisible = true;
      this.$nextTick((_) => {
        this.$refs.saveTagInput.$refs.input.focus();
      });
    },
    handleInputConfirm() {
      console.log(1);
      let inputValue = this.inputValue;
      if (inputValue) {
        this.dynamicTags.push(inputValue);
        this.dataForm.keyword = this.dynamicTags.join();
        console.log(this.dataForm.keyword);
      }
      this.inputVisible = false;
      this.inputValue = "";
    },
    init(data, id) {
      if (data) {
        this.dataForm = data;
        this.dynamicTags = data.keyword.split(",");
      } else {
        (this.dataForm.supplyStatus = ""),
          (this.dataForm.supplyStatus = ""),
          (this.dataForm.mainSpecifications = ""),
          (this.dataForm.heatTreatment = ""),
          (this.dataForm.chemicalComposition = ""),
          (this.dataForm.meltingProcess = ""),
          (this.dataForm.standard = ""),
          (this.dataForm.overview = ""),
          (this.dataForm.reference = ""),
          (this.dataForm.standard = ""),
          (this.dataForm.source = ""),
          (this.dataForm.keyword = ""),
          (this.dataForm.code = ""),
          (this.dataForm.author = ""),
          (this.dataForm.image = "");
        this.dynamicTags = [];
        this.dataForm.categoryId = id;
      }
      this.image = this.$http.adornUrl(
        `/sys/filemanager/uploadimg?token=${this.$cookie.get("token")}`
      );
    },
    handlePictureCardPreview(response,file,fileList) {
      this.dataForm.image = response.filename;
      // console.log(this.image);
    },
    handErr(){
      this.$message.error("请删除已有已有文件")
    }
  },
  watch: {},
};
</script>
<style scoped>
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

.el-tag + .el-tag {
  margin-left: 10px;
}
.button-new-tag {
  margin-left: 10px;
  height: 32px;
  line-height: 30px;
  padding-top: 0;
  padding-bottom: 0;
}
.input-new-tag {
  width: 90px;
  margin-left: 10px;
  vertical-align: bottom;
}
</style>

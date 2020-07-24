<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="120px" :rules="dataRule">
      <el-form-item label="标题图" prop="image">
        <!-- <el-input v-model="dataForm.image" placeholder="标题图"></el-input> -->
        <el-upload
          class="avatar-uploader"
          action="https://jsonplaceholder.typicode.com/posts/"
          :show-file-list="false"
        >
          <img v-if="imageUrl" :src="imageUrl" class="avatar" />
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </el-form-item>

      <!-- <el-form-item label="材料图片" prop="code">
        <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
      </el-form-item>-->
      <el-form-item label="材料牌号" prop="code">
        <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
      </el-form-item>
      <!-- <el-form-item label="材料类型id" prop="categoryId">
        <el-input v-model="dataForm.categoryId" placeholder="材料类型id"></el-input>
      </el-form-item>-->
      <el-form-item label="关键词" prop="keyword">
        <!-- <el-input v-model="dataForm.keyword" placeholder="关键词"></el-input> -->
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
      <el-form-item label="参考文献" prop="references">
        <el-input v-model="dataForm.references" placeholder="参考文献" type="textarea"></el-input>
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
      imageUrl: "",
      dataForm: {
        categoryId: "",
        supplyStatus: "",
        mainSpecifications: "",
        heatTreatment: "",
        chemicalComposition: "",
        meltingProcess: "",
        standard: "",
        overview: "",
        references: "",
        standard: "",
        source: "",
        keyword: "",
        code: "",
        author: "",
        imageUrl: "",
      },
      dataRule: {
        code: [
          { required: true, message: "材料牌号不能为空", trigger: "blur" },
        ],
        categoryId: [
          { required: true, message: "材料类型id不能为空", trigger: "blur" },
        ],
        image: [{ required: true, message: "标题图不能为空", trigger: "blur" }],
        references: [
          { required: true, message: "参考文献不能为空", trigger: "blur" },
        ],
        source: [{ required: true, message: "来源不能为空", trigger: "blur" }],
        author: [{ required: true, message: "作者不能为空", trigger: "blur" }],
        overview: [
          { required: true, message: "材料概述不能为空", trigger: "blur" },
        ],
        standard: [
          { required: true, message: "材料标准不能为空", trigger: "blur" },
        ],
        meltingProcess: [
          { required: true, message: "熔炼工艺不能为空", trigger: "blur" },
        ],
        chemicalComposition: [
          { required: true, message: "化学成分不能为空", trigger: "blur" },
        ],
        heatTreatment: [
          { required: true, message: "热处理制度不能为空", trigger: "blur" },
        ],
        mainSpecifications: [
          { required: true, message: "熔炼工艺不能为空", trigger: "blur" },
        ],
        supplyStatus: [
          { required: true, message: "供应状态不能为空", trigger: "blur" },
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
      console.log(1)
    },
    showInput() {
      this.inputVisible = true;
      this.$nextTick((_) => {
        this.$refs.saveTagInput.$refs.input.focus();
      });
    },
    handleInputConfirm() {
      let inputValue = this.inputValue;
      if (inputValue) {
        this.dynamicTags.push(inputValue);
        this.dataForm.keyword = this.dynamicTags
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
          (this.dataForm.references = ""),
          (this.dataForm.standard = ""),
          (this.dataForm.source = ""),
          (this.dataForm.keyword = ""),
          (this.dataForm.code = ""),
          (this.dataForm.author = ""),
          (this.dataForm.imageUrl = "");
        this.dynamicTags = [];
        this.dataForm.categoryId = id;
      }
    },
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

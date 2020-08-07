<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="140px">
      <el-form-item label="技术标准规范的性能" prop="technicalStandardSpecifications">
        <el-input
          v-model="dataForm.technicalStandardSpecifications"
          placeholder="技术标准规范的性能"
          type="textarea"
        ></el-input>
      </el-form-item>
      <el-form-item label="生产检验数据" prop="productionInspection">
        <el-input v-model="dataForm.productionInspection" placeholder="生产检验数据" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="硬 度" prop="hardness">
        <el-input v-model="dataForm.hardness" placeholder="硬 度" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="冲击性能" prop="impactPerformance">
        <el-input v-model="dataForm.impactPerformance" placeholder="冲击性能" type="textarea"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "physicalData",
  data() {
    return {
      imageUrl: "",
      dataForm: {
        categoryId: "",
        technicalStandardSpecifications: "",
        productionInspection: "",
        hardness: "",
        impactPerformance: "",
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
        this.dataForm.technicalStandardSpecifications = "";
        this.dataForm.productionInspection = "";
        this.dataForm.hardness = "";
        this.dataForm.impactPerformance = "";
      }
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


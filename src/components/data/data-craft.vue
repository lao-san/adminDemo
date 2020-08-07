<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="140px">
      <el-form-item label="成形工艺与性能" prop="processAndPerformance">
        <el-input
          v-model="dataForm.processAndPerformance"
          placeholder="成形工艺与性能"
          type="textarea"
        ></el-input>
      </el-form-item>
      <el-form-item label="焊接性能" prop="weldingPerformance">
        <el-input v-model="dataForm.weldingPerformance" placeholder="焊接性能" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="零件热处理工艺" prop="heatTreatmentProcess">
        <el-input v-model="dataForm.heatTreatmentProcess" placeholder="零件热处理工艺" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="切削加工与磨削性能" prop="cuttingAndGrinding">
        <el-input v-model="dataForm.cuttingAndGrinding" placeholder="切削加工与磨削性能" type="textarea"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "dataCraft",
  data() {
    return {
      imageUrl: "",
      dataForm: {
        categoryId: "",
        processAndPerformance:"",
        weldingPerformance:"",
        heatTreatmentProcess:"",
        cuttingAndGrinding:""
      
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
        this.dataForm.processAndPerformance = "";
        this.dataForm.weldingPerformance = "";
        this.dataForm.heatTreatmentProcess = "";
        this.dataForm.cuttingAndGrinding = "";
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


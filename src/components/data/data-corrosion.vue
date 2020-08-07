<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="100px">
      <el-form-item label="抗氧化性能" prop="antioxidantPerformance">
        <el-input v-model="dataForm.antioxidantPerformance" placeholder="抗氧化性能" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="腐蚀性能" prop="meltingProcess">
        <el-input v-model="dataForm.meltingProcess" placeholder="腐蚀性能" type="textarea"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: "dataCorrosion",
  data() {
    return {
      imageUrl: "",
      dataForm: {
        categoryId: "",
        antioxidantPerformance: "",
        meltingProcess: "",
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
        this.dataForm.antioxidantPerformance = "";
        this.dataForm.meltingProcess = "";
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


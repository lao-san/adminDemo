<template>
  <div>
    <el-form ref="form" :model="dataForm" label-width="140px">
      <el-form-item label="典型组织" prop="typicalOrganization">
        <script
          :id="ueId"
          class="ueditor-box"
          type="text/plain"
          style="width: 100%; height: 360px;"
        ></script>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import ueditor from "ueditor";

export default {
  name: "dataStructure",
  data() {
    return {
      imageUrl: "",
      dataForm: {
        categoryId: "",
        typicalOrganization: "",
      },
      inputVisible: false,
      ueId: `J_ueditorBox_${new Date().getTime()}`,
    };
  },
  components: {},
  computed: {},
  beforeMount() {},
  mounted() {
    this.ue = ueditor.getEditor(this.ueId, {
      // serverUrl: '', // 服务器统一请求接口路径
      zIndex: 3000,
    });
  },
  methods: {
    init(data, id) {
      // ue.execCommand('clearlocaldata');

      if (data.typicalOrganization) {
        this.dataForm = data;
        this.ue.ready(() => {
          this.ue.setContent(data.typicalOrganization);
        });
      } else {
        this.ue.execCommand("cleardoc");
        this.dataForm.categoryId = id;
        this.dataForm.typicalOrganization = "";
      }
    },
    getContent() {
      this.ue.ready(() => {
        this.dataForm.typicalOrganization = this.ue.getContent();
      });
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


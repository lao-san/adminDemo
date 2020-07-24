<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmit()"
      label-width="80px"
    >
      <el-form-item label="材料牌号" prop="code">
        <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
      </el-form-item>
      <el-form-item label="典型组织" prop="typicalOrganization">
        <el-input v-model="dataForm.typicalOrganization" placeholder="典型组织"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        dId: 0,
        code: "",
        typicalOrganization: "",
      },
      dataRule: {
        code: [
          { required: true, message: "材料牌号不能为空", trigger: "blur" },
        ],
        typicalOrganization: [
          { required: true, message: "典型组织不能为空", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    init(id) {
      this.dataForm.dId = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.dId) {
          this.$http({
            url: this.$http.adornUrl(
              `/admin/datastructure/info/${this.dataForm.dId}`
            ),
            method: "get",
            params: this.$http.adornParams(),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.code = data.dataStructure.code;
              this.dataForm.typicalOrganization =
                data.dataStructure.typicalOrganization;
            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate((valid) => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(
              `/admin/datastructure/${!this.dataForm.dId ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              dId: this.dataForm.dId || undefined,
              code: this.dataForm.code,
              typicalOrganization: this.dataForm.typicalOrganization,
            }),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                },
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    },
  },
};
</script>

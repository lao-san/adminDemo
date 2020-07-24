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
      label-width="120px"
    >
      <el-form-item label="父id" prop="parentId">
        <el-cascader
          v-model="dataForm.parentId"
          :options="options"
          :props="defaultParams"
          @change="handleChange"
        ></el-cascader>
      </el-form-item>
      <el-form-item label="材料类型名" prop="name">
        <el-input v-model="dataForm.name" placeholder="材料类型名"></el-input>
      </el-form-item>
      <el-form-item label="url路径" prop="url">
        <el-input v-model="dataForm.url" placeholder="url路径"></el-input>
      </el-form-item>
      <el-form-item label="是否为终极栏目" prop="last">
        <el-radio-group v-model="dataForm.last">
          <el-radio :label="1">是</el-radio>
          <el-radio :label="0">否</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="排序" prop="orderNum">
        <el-input v-model="dataForm.orderNum" placeholder="排序"></el-input>
      </el-form-item>
      <el-form-item label="备注" prop="remark">
        <el-input
          type="textarea"
          :autosize="{ minRows: 6, maxRows: 8}"
          v-model="dataForm.remark"
          placeholder="备注"
        ></el-input>
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
      value: [],
      visible: false,
      dataForm: {
        id: 0,
        parentId: "",
        name: "",
        tablename: "",
        url: "",
        last: "",
        remark: "",
        orderNum: "",
      },
      dataRule: {
        parentId: [
          { required: true, message: "父id不能为空", trigger: "blur" },
        ],
      },
      defaultParams: {
        checkStrictly: true,
        label: "name",
        value: "id",
        children: "list",
      },
      options: [],
    };
  },
  mounted() {
    this.getCategory();
  },
  methods: {
    init(id) {
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.$http({
            url: this.$http.adornUrl(
              `/admin/category/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams(),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.parentId = data.category.parentId;
              this.dataForm.name = data.category.name;
              this.dataForm.tablename = data.category.tablename;
              this.dataForm.url = data.category.url;
              this.dataForm.last = data.category.last;
              this.dataForm.remark = data.category.remark;
              this.dataForm.orderNum = data.category.orderNum;
              this.dataForm.createTime = data.category.createTime;
              this.dataForm.isDel = data.category.isDel;
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
              `/admin/category/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              parentId: this.dataForm.parentId,
              name: this.dataForm.name,
              tablename: this.dataForm.tablename,
              url: this.dataForm.url,
              last: this.dataForm.last,
              remark: this.dataForm.remark,
              orderNum: this.dataForm.orderNum,
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
    getCategory() {
      this.$http({
        url: this.$http.adornUrl("/admin/category/selecttree"),
        method: "get",
        params: this.$http.adornParams(),
      }).then(({ data }) => {
        console.log(data.categoryList);
        this.options = this.getTreeData(data.categoryList);
      });
    },
    // 获取树形数据
    getTreeData(data) {
      // 循环遍历json数据
      for (var i = 0; i < data.length; i++) {
        if (data[i].list == null || data[i].list.length < 1) {
          // children若为空数组，则将children设为undefined
          data[i].list = undefined;
        } else {
          // children若不为空数组，则继续 递归调用 本方法
          this.getTreeData(data[i].list);
        }
      }
      return data;
    },
    handleChange(data) {
      this.dataForm.parentId = data[data.length - 1];
    },
  },
};
</script>

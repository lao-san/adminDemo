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
      label-width="140px"
    >
      <el-row>
        <el-col :span="12">
          <el-form-item label="材料名称" prop="name">
            <el-input v-model="dataForm.name" placeholder="材料名称"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="材料类别" >
            <el-cascader
              v-model="value"
              :options="options"
              style="margin-left:10px"
              :props="defaultParams"
            ></el-cascader>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="材料体系" prop="system">
            <el-input v-model="dataForm.system" placeholder="材料体系"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="材料牌号" prop="code">
            <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="技术条件" prop="technicalConditions">
            <el-input v-model="dataForm.technicalConditions" placeholder="技术条件"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="生产厂家" prop="manufacturer">
            <el-input v-model="dataForm.manufacturer" placeholder="生产厂家"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="零件名称" prop="partName">
            <el-input v-model="dataForm.partName" placeholder="零件名称"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="主要功能" prop="mainFunction">
            <el-input v-model="dataForm.mainFunction" placeholder="主要功能"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="应用部位" prop="applicationSite">
            <el-input v-model="dataForm.applicationSite" placeholder="应用部位"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="工况环境" prop="workingEnvironment">
            <el-input v-model="dataForm.workingEnvironment" placeholder="工况环境"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="失效模式与机理" prop="failureModeMechanism">
            <el-input v-model="dataForm.failureModeMechanism" placeholder="失效模式与机理"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="保管期/年" prop="storageLife">
            <el-input v-model="dataForm.storageLife" placeholder="保管期/年"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="周转期/年" prop="turnoverLife">
            <el-input v-model="dataForm.turnoverLife" placeholder="周转期/年"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="贮存期/年" prop="shelfLife">
            <el-input v-model="dataForm.shelfLife" placeholder="贮存期/年"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="型号使用信息" prop="usageInformation">
            <el-input v-model="dataForm.usageInformation" placeholder="型号使用信息"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item label="备注" prop="remark">
        <el-input v-model="dataForm.remark" placeholder="备注" type="textarea"></el-input>
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
      value: "",
      visible: false,
      options: [],
      defaultParams: {
        label: "name",
        value: "id",
        children: "list",
      },
      dataForm: {
        id: 0,
        categoryId: "",
        system: "",
        name: "",
        code: "",
        technicalConditions: "",
        manufacturer: "",
        partName: "",
        mainFunction: "",
        applicationSite: "",
        workingEnvironment: "",
        failureModeMechanism: "",
        storageLife: "",
        turnoverLife: "",
        shelfLife: "",
        usageInformation: "",
        remark: "",
      },
      dataRule: {
       
        system: [
          { required: true, message: "材料体系不能为空", trigger: "blur" },
        ],
        name: [
          { required: true, message: "材料名称不能为空", trigger: "blur" },
        ],
        code: [
          { required: true, message: "牌号名称不能为空", trigger: "blur" },
        ],
       
      },
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
              `/admin/materialstorage/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams(),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.categoryId = data.materialStorage.categoryId;
              this.dataForm.system = data.materialStorage.system;
              this.dataForm.name = data.materialStorage.name;
              this.dataForm.code = data.materialStorage.code;
              this.dataForm.technicalConditions =
                data.materialStorage.technicalConditions;
              this.dataForm.manufacturer = data.materialStorage.manufacturer;
              this.dataForm.partName = data.materialStorage.partName;
              this.dataForm.mainFunction = data.materialStorage.mainFunction;
              this.dataForm.applicationSite =
                data.materialStorage.applicationSite;
              this.dataForm.workingEnvironment =
                data.materialStorage.workingEnvironment;
              this.dataForm.failureModeMechanism =
                data.materialStorage.failureModeMechanism;
              this.dataForm.storageLife = data.materialStorage.storageLife;
              this.dataForm.turnoverLife = data.materialStorage.turnoverLife;
              this.dataForm.shelfLife = data.materialStorage.shelfLife;
              this.dataForm.usageInformation =
                data.materialStorage.usageInformation;
              this.dataForm.remark = data.materialStorage.remark;
              this.value =data.materialStorage.categoryId;
            }
          });
        } else {
          this.value = '';
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate((valid) => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(
              `/admin/materialstorage/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              categoryId: this.value[this.value.length - 1] || this.value,
              system: this.dataForm.system,
              name: this.dataForm.name,
              code: this.dataForm.code,
              technicalConditions: this.dataForm.technicalConditions,
              manufacturer: this.dataForm.manufacturer,
              partName: this.dataForm.partName,
              mainFunction: this.dataForm.mainFunction,
              applicationSite: this.dataForm.applicationSite,
              workingEnvironment: this.dataForm.workingEnvironment,
              failureModeMechanism: this.dataForm.failureModeMechanism,
              storageLife: this.dataForm.storageLife,
              turnoverLife: this.dataForm.turnoverLife,
              shelfLife: this.dataForm.shelfLife,
              usageInformation: this.dataForm.usageInformation,
              remark: this.dataForm.remark,
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
        url: this.$http.adornUrl("/admin/category/treelist"),
        method: "get",
        params: this.$http.adornParams(),
      }).then(({ data }) => {
        this.options = this.getTreeData(data.list);
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
    }
  },
};
</script>
<style lang="scss" scoped>
.el-cascader {
  margin-left: 0px !important;
}
</style>

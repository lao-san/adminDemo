<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible"
  >
    <el-tabs tab-position="top">
      <el-tab-pane label="合金介绍">
        <basics ref="basics"></basics>
      </el-tab-pane>
    </el-tabs>
    <!-- 
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
      <el-form-item label="材料类型id" prop="categoryId">
        <el-input v-model="dataForm.categoryId" placeholder="材料类型id"></el-input>
      </el-form-item>
      <el-form-item label="关键词" prop="keyword">
        <el-input v-model="dataForm.keyword" placeholder="关键词"></el-input>
      </el-form-item>
      <el-form-item label="标题图" prop="image">
        <el-input v-model="dataForm.image" placeholder="标题图"></el-input>
      </el-form-item>
      <el-form-item label="参考文献" prop="references">
        <el-input v-model="dataForm.references" placeholder="参考文献"></el-input>
      </el-form-item>
      <el-form-item label="来源" prop="source">
        <el-input v-model="dataForm.source" placeholder="来源"></el-input>
      </el-form-item>
      <el-form-item label="作者" prop="author">
        <el-input v-model="dataForm.author" placeholder="作者"></el-input>
      </el-form-item>
      <el-form-item label="材料概述" prop="overview">
        <el-input v-model="dataForm.overview" placeholder="材料概述"></el-input>
      </el-form-item>
      <el-form-item label="材料标准" prop="standard">
        <el-input v-model="dataForm.standard" placeholder="材料标准"></el-input>
      </el-form-item>
      <el-form-item label="熔炼工艺" prop="meltingProcess">
        <el-input v-model="dataForm.meltingProcess" placeholder="熔炼工艺"></el-input>
      </el-form-item>
      <el-form-item label="化学成分" prop="chemicalComposition">
        <el-input v-model="dataForm.chemicalComposition" placeholder="化学成分"></el-input>
      </el-form-item>
      <el-form-item label="热处理制度" prop="heatTreatment">
        <el-input v-model="dataForm.heatTreatment" placeholder="热处理制度"></el-input>
      </el-form-item>
      <el-form-item label="熔炼工艺" prop="mainSpecifications">
        <el-input v-model="dataForm.mainSpecifications" placeholder="熔炼工艺"></el-input>
      </el-form-item>
      <el-form-item label="供应状态" prop="supplyStatus">
        <el-input v-model="dataForm.supplyStatus" placeholder="供应状态"></el-input>
      </el-form-item>
    </el-form>-->
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
import Basics from "../../../components/data/basics1";

export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: 0,
        code: "",
        categoryId: "",
        keyword: "",
        image: "",
        references: "",
        source: "",
        author: "",
        overview: "",
        standard: "",
        meltingProcess: "",
        chemicalComposition: "",
        heatTreatment: "",
        mainSpecifications: "",
        supplyStatus: "",
      },
      dataRule: {
        code: [
          { required: true, message: "材料牌号不能为空", trigger: "blur" },
        ],
        categoryId: [
          { required: true, message: "材料类型id不能为空", trigger: "blur" },
        ],
        keyword: [
          { required: true, message: "关键词不能为空", trigger: "blur" },
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
    };
  },
  methods: {
    init(id, tabId) {
      console.log(tabId);
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        // this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.$http({
            url: this.$http.adornUrl(`/admin/data/info/${this.dataForm.id}`),
            method: "get",
            params: this.$http.adornParams(),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$refs.basics.init(data.data.dataBase,tabId);
            }
          });
        }else {
           this.$refs.basics.init("",tabId);
           console.log(this.$refs.basics.dataForm)
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate((valid) => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(
              `/admin/database/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              code: this.dataForm.code,
              categoryId: this.dataForm.categoryId,
              keyword: this.dataForm.keyword,
              image: this.dataForm.image,
              references: this.dataForm.references,
              source: this.dataForm.source,
              author: this.dataForm.author,
              overview: this.dataForm.overview,
              standard: this.dataForm.standard,
              meltingProcess: this.dataForm.meltingProcess,
              chemicalComposition: this.dataForm.chemicalComposition,
              heatTreatment: this.dataForm.heatTreatment,
              mainSpecifications: this.dataForm.mainSpecifications,
              supplyStatus: this.dataForm.supplyStatus,
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
  components: {
    Basics,
  },
  mounted(){
   
  }
};
</script>

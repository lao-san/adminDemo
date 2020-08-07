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
      <el-tab-pane label="物理性能">
        <physical-data ref="physical"></physical-data>
      </el-tab-pane>
      <el-tab-pane label="力学性能">
        <data-mechanics ref="mechanics"></data-mechanics>
      </el-tab-pane>
      <el-tab-pane label="工艺要求">
        <data-craft ref="craft"></data-craft>
      </el-tab-pane>
      <el-tab-pane label="组织结构">
        <data-structure ref="structure"></data-structure>
      </el-tab-pane>
      <el-tab-pane label="防腐性">
        <data-corrosion ref="corrosion"></data-corrosion>
      </el-tab-pane>
    </el-tabs>

    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
import Basics from "../../../components/data/basics1";
import PhysicalData from "../../../components/data/physical-data";
import DataMechanics from "../../../components/data/data-mechanics";
import DataCraft from "../../../components/data/data-craft";
import DataStructure from "../../../components/data/data-structure";
import DataCorrosion from "../../../components/data/data-corrosion";

export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: 0,
        code: "",
        categoryId: "",
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
      // console.log(tabId);
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
              this.$refs.basics.init(data.data.dataBase, tabId);
              this.$refs.physical.init(data.data.dataPhysical, tabId);
              this.$refs.mechanics.init(data.data.dataMechanics, tabId);
              this.$refs.craft.init(data.data.dataCraft, tabId);
              this.$refs.corrosion.init(data.data.dataCorrosion, tabId);
              this.$refs.structure.init(data.data.dataStructure, tabId);
            }
          });
        } else {
          this.$refs.basics.init("", tabId);
          this.$refs.physical.init("", tabId);
          this.$refs.mechanics.init("", tabId);
          this.$refs.craft.init("", tabId);
          this.$refs.corrosion.init("", tabId);
          this.$refs.structure.init("", tabId);
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs.structure.getContent();
      this.$http({
        url: this.$http.adornUrl(
          `/admin/data/${!this.dataForm.id ? "save" : "update"}`
        ),
        method: "post",
        data: this.$http.adornData({
          dataBase: this.$refs.basics.dataForm,
          dataPhysical: this.$refs.physical.dataForm,
          dataMechanics: this.$refs.mechanics.dataForm,
          dataCraft: this.$refs.craft.dataForm,
          dataStructure: this.$refs.structure.dataForm,
          dataCorrosion: this.$refs.corrosion.dataForm,
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
        // console.log(this.$refs.basics.dataForm);
      });
    },
  },
  components: {
    Basics,
    PhysicalData,
    DataMechanics,
    DataCraft,
    DataStructure,
    DataCorrosion,
  },
  mounted() {},
};
</script>

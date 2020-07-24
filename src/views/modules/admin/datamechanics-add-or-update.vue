<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="材料牌号" prop="code">
      <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
    </el-form-item>
    <el-form-item label="技术标准规范的性能" prop="technicalStandardSpecifications">
      <el-input v-model="dataForm.technicalStandardSpecifications" placeholder="技术标准规范的性能"></el-input>
    </el-form-item>
    <el-form-item label="生产检验数据" prop="productionInspection">
      <el-input v-model="dataForm.productionInspection" placeholder="生产检验数据"></el-input>
    </el-form-item>
    <el-form-item label="硬度" prop="hardness">
      <el-input v-model="dataForm.hardness" placeholder="硬度"></el-input>
    </el-form-item>
    <el-form-item label="冲击性能" prop="impactPerformance">
      <el-input v-model="dataForm.impactPerformance" placeholder="冲击性能"></el-input>
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
    data () {
      return {
        visible: false,
        dataForm: {
          dId: 0,
          code: '',
          technicalStandardSpecifications: '',
          productionInspection: '',
          hardness: '',
          impactPerformance: ''
        },
        dataRule: {
          code: [
            { required: true, message: '材料牌号不能为空', trigger: 'blur' }
          ],
          technicalStandardSpecifications: [
            { required: true, message: '技术标准规范的性能不能为空', trigger: 'blur' }
          ],
          productionInspection: [
            { required: true, message: '生产检验数据不能为空', trigger: 'blur' }
          ],
          hardness: [
            { required: true, message: '硬度不能为空', trigger: 'blur' }
          ],
          impactPerformance: [
            { required: true, message: '冲击性能不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.dId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.dId) {
            this.$http({
              url: this.$http.adornUrl(`/admin/datamechanics/info/${this.dataForm.dId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.code = data.dataMechanics.code
                this.dataForm.technicalStandardSpecifications = data.dataMechanics.technicalStandardSpecifications
                this.dataForm.productionInspection = data.dataMechanics.productionInspection
                this.dataForm.hardness = data.dataMechanics.hardness
                this.dataForm.impactPerformance = data.dataMechanics.impactPerformance
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/admin/datamechanics/${!this.dataForm.dId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'dId': this.dataForm.dId || undefined,
                'code': this.dataForm.code,
                'technicalStandardSpecifications': this.dataForm.technicalStandardSpecifications,
                'productionInspection': this.dataForm.productionInspection,
                'hardness': this.dataForm.hardness,
                'impactPerformance': this.dataForm.impactPerformance
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>

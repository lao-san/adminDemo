<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="材料牌号" prop="code">
      <el-input v-model="dataForm.code" placeholder="材料牌号"></el-input>
    </el-form-item>
    <el-form-item label="成形工艺与性能" prop="processAndPerformance">
      <el-input v-model="dataForm.processAndPerformance" placeholder="成形工艺与性能"></el-input>
    </el-form-item>
    <el-form-item label="焊接性能" prop="weldingPerformance">
      <el-input v-model="dataForm.weldingPerformance" placeholder="焊接性能"></el-input>
    </el-form-item>
    <el-form-item label="零件热处理工艺" prop="heatTreatmentProcess">
      <el-input v-model="dataForm.heatTreatmentProcess" placeholder="零件热处理工艺"></el-input>
    </el-form-item>
    <el-form-item label="切削加工与磨削性能" prop="cuttingAndGrinding">
      <el-input v-model="dataForm.cuttingAndGrinding" placeholder="切削加工与磨削性能"></el-input>
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
          processAndPerformance: '',
          weldingPerformance: '',
          heatTreatmentProcess: '',
          cuttingAndGrinding: ''
        },
        dataRule: {
          code: [
            { required: true, message: '材料牌号不能为空', trigger: 'blur' }
          ],
          processAndPerformance: [
            { required: true, message: '成形工艺与性能不能为空', trigger: 'blur' }
          ],
          weldingPerformance: [
            { required: true, message: '焊接性能不能为空', trigger: 'blur' }
          ],
          heatTreatmentProcess: [
            { required: true, message: '零件热处理工艺不能为空', trigger: 'blur' }
          ],
          cuttingAndGrinding: [
            { required: true, message: '切削加工与磨削性能不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/admin/datacraft/info/${this.dataForm.dId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.code = data.dataCraft.code
                this.dataForm.processAndPerformance = data.dataCraft.processAndPerformance
                this.dataForm.weldingPerformance = data.dataCraft.weldingPerformance
                this.dataForm.heatTreatmentProcess = data.dataCraft.heatTreatmentProcess
                this.dataForm.cuttingAndGrinding = data.dataCraft.cuttingAndGrinding
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
              url: this.$http.adornUrl(`/admin/datacraft/${!this.dataForm.dId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'dId': this.dataForm.dId || undefined,
                'code': this.dataForm.code,
                'processAndPerformance': this.dataForm.processAndPerformance,
                'weldingPerformance': this.dataForm.weldingPerformance,
                'heatTreatmentProcess': this.dataForm.heatTreatmentProcess,
                'cuttingAndGrinding': this.dataForm.cuttingAndGrinding
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

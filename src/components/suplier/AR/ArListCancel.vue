<template>
    <section>
        <dialog-info :visible-p.sync="dialogInfoVisible" :details-p="details"></dialog-info>
        <el-table
        :data="dataTable"
        show-summary
        border
        sum-text="本页合计"
        style="width: 100%"
        :row-class-name="tableRowClassName" @expand-change="expendhandle">
        <el-table-column align="center"
        fixed
        type="index"
        label="序号"
        >
        </el-table-column>
        <el-table-column align="center"
        fixed
        label="AR单号"
        prop="masterChainId"
        sortable
        min-width="130">
        </el-table-column>
        <el-table-column align="center"
        label="付款单位"
        prop="come"
        >
        </el-table-column>
        <el-table-column align="center"
        label="授让单位"
        prop="custFromName"
        >
        </el-table-column>
        <el-table-column align="center"
        label="交易流水号"
        sortable
        min-width="120"
        prop="transSerialNo"
        >
        </el-table-column>
        <el-table-column align="center"
        label="状态"
        prop="arStatusTypeName"
        >
        </el-table-column>
        <el-table-column align="center"
        label="币别"
        prop="currencyDesc"
        >
        </el-table-column>
        <el-table-column align="center"
        label="票面金额"
        prop="transAmt"
        >
        </el-table-column><el-table-column align="center"
        label="转让日期"
        sortable
        prop="transDate"
        :formatter="dateFormat"
        min-width="120">
        </el-table-column>
        <el-table-column align="center"
        label="预计回款日期"
        prop="billPayDate"
        :formatter="dateFormat"
        min-width="120"
        >
        </el-table-column>
        <el-table-column align="center"
        fixed="right"
        label="操作"
        width='230px'
        >
        <template slot-scope="scope" >
            <el-button
            size="mini"
            type="primary"
            @click="handleInfo(scope.$index, scope.row)">详情</el-button>
            <el-button
            size="mini"
            type="primary"
            @click="handleCancle(scope.$index, scope.row)" v-if="scope.row.checkedStatus==3">取消授让</el-button>
        </template>
        </el-table-column>
    </el-table>
    </section>
</template>

<script>
import ListMinxIn from '@/mixins/suplier/Ar/List'
import Common from '@/mixins/common'
import Dialog from '@/mixins/suplier/Ar/Dialog'
/* 取消转让列表 */
export default {
  props: ['dataLoading', 'dataTable'],
  mixins: [Common, ListMinxIn, Dialog],
  components: {
    'dialog-info': () =>
      import(/* webpackChunkName: 'Dialog' */ '@/components/suplier/Ar/DialogInfoCancelTrans')
  },
  methods: {
    handleCancle (idx, val) {
      this.$confirm(`单号为${val.masterChainId}的确认取消其授让?`, `${val.masterChainId}取消授让`, {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.cancelBase('/transferedAr/cancelTranfered.do', val.masterChainId)
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '取消'
        })
      })
    }
  }
}
</script>
<style>
.demo-table-expand label {
  width: 100px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
</style>

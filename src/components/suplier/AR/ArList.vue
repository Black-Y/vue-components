<template>
  <div>
    <dialog-info :visible-p.sync="dialogInfoVisible" :details-p="details"></dialog-info>
    <section>
    <el-table :data="dataTable" v-loading="dataLoading"  element-loading-text="拼命加载中" element-loading-spinner="el-icon-loading"
        element-loading-background="rgba(0, 0, 0, 0.8)" border show-summary :summary-method="sumHandle([7])" sum-text="本页合计" style="width: 100%" :row-class-name="tableRowClassName"
      @expand-change="expendhandle">
      <el-table-column align="center" fixed type="index" label="序号" width="60">
      </el-table-column>
      <el-table-column align="center" fixed sortable label="AR单号" prop="masterChainId" width="130">
      </el-table-column>
      <el-table-column align="center" label="付款单位" prop="companyName">
      </el-table-column>
      <el-table-column align="center" label="授让单位" prop="custFromName">
      </el-table-column>
      <el-table-column align="center" sortable label="交易流水号" prop="transSerialNo" min-width="120">
      </el-table-column>
      <el-table-column align="center" label="状态" prop="arStatusTypeName">
      </el-table-column>
      <el-table-column align="center" label="币别" prop="currencyDesc">
      </el-table-column>
      <el-table-column align="center" label="票面金额" prop="transAmt">
      </el-table-column>
      <el-table-column align="center" label="转让日期" sortable prop="transDate" min-width="120" :formatter="dateFormat">
      </el-table-column>
      <el-table-column align="center" label="预计回款日期" prop="billPayDate" min-width="120" :formatter="dateFormat">
      </el-table-column>
      <el-table-column align="center" label="操作" width='230px' fixed="right">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="handleInfo(scope.$index, scope.row)">详情</el-button>
          <el-button v-if="scope.row.checkedStatus==3" size="mini" type="primary" @click="handleAccept(scope.$index, scope.row)">接受</el-button>
          <el-button v-if="scope.row.checkedStatus==3" size="mini" type="primary" @click="handleReject(scope.$index, scope.row)">拒绝</el-button>
        </template>
      </el-table-column>
    </el-table>
  </section>
  </div>
</template>

<script>
import ListMinxIn from '@/mixins/suplier/Ar/List'
import Common from '@/mixins/common'
import Dialog from '@/mixins/suplier/Ar/Dialog'
/* 待收Ar列表 */
export default {
  props: ['dataLoading', 'dataTable'],
  mixins: [ListMinxIn, Common, Dialog],
  components: {
    'dialog-info': () =>
      import(/* webpackChunkName: 'Dialog' */ '@/components/suplier/Ar/DialogInfo')
  },
  methods: {
    handleAccept (idx, val) {
      this.$confirm(`单号为${val.masterChainId}的确认接授其转让?`, `${val.masterChainId}接收转让`, {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.cancelBase('/onReceivingAr/receiveTranfer.do', val.masterChainId)
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '取消'
        })
      })
    },
    handleReject (idx, val) {
      this.$confirm(`单号为${val.masterChainId}的确认拒绝其转让?`, `${val.masterChainId}拒绝转让`, {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.cancelBase('/onReceivingAr/rejectTranfer.do', val.masterChainId)
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

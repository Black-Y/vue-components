<template>
  <div>
    <dialog-info :visible-p.sync="dialogInfoVisible" :details-p="details"></dialog-info>
    <dialog-contract :visible-p.sync="dialogTransferVisible" :details-p="details"></dialog-contract>
    <section>
    <el-table :data="dataTable" v-loading="dataLoading"  element-loading-text="拼命加载中" element-loading-spinner="el-icon-loading"
        element-loading-background="rgba(0, 0, 0, 0.8)" border show-summary :summary-method="sumHandle([5,6])" sum-text="本页合计" style="width: 100%" :row-class-name="tableRowClassName"
      @expand-change="expendhandle">
      <el-table-column align="center" fixed type="index" label="序号" width="60">
      </el-table-column>
      <el-table-column align="center" fixed sortable label="AR单号" prop="masterChainId" width="130">
      </el-table-column>
      <el-table-column align="center" label="结报单号" prop="billId">
      </el-table-column>
      <el-table-column align="center" label="贴现客户" prop="custFromName">
      </el-table-column>
      <el-table-column align="center" label="贴现金额" prop="billBookAmt">
      </el-table-column>
      <el-table-column align="center" label="币别" prop="currencyDesc">
      </el-table-column>
      <el-table-column align="center" label="付款单位" prop="companyName" >
      </el-table-column>
      <el-table-column align="center" label="预计回款日期" prop="billPayDate" min-width="120" :formatter="dateFormat">
      </el-table-column>
      <el-table-column align="center" label="操作" width='230px' fixed="right">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="handleInfo(scope.$index, scope.row)">详情</el-button>
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
export default {
  props: ['dataLoading', 'dataTable'],
  data () {
    return {
    }
  },
  mixins: [ListMinxIn, Common, Dialog],
  components: {
    'dialog-info': () =>
      import(/* webpackChunkName: 'Dialog' */ '@/components/Fund/Work/DialogInfoLoanReject')
  },
  methods: {
    // 详情
    handleInfo (idx, val) {
      val.infoLoading = true
      this.getLoanDetail('/loan2/queryLoanInfo.do', { masterChainId: val.masterChainId }).then(res => {
        this.details = res
        this.dialogInfoVisible = true
        val.infoLoading = false
      })
    }
  }
}

</script>
<style lang="scss">
.demo-table-expand label {
  width: 100px;
  color: #99a9bf;
}

.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
 /* 更多菜单样式 */
.el-dropdown {
  margin-left: 20px;
}
.el-dropdown-link {
  cursor: pointer;
  color: #033c81;
}
.el-icon-arrow-down {
  font-size: 12px;
}
.el-dropdown-menu.el-popper {
  li:not(:first-of-type) {
    margin-top: 10px;
  }
  .full-width {
    width: 100%;
  }
}
</style>

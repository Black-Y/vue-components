<template>
  <div class="main">
    <div class="body">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <img src="~@/assets/img/juxin_06.png" alt="查询条件">
          <span>查询条件</span>
        </div>
        <search @handle-search="handleSearch"></search>
      </el-card>
    </div>
    <div class="body">
      <el-card class="box-card">
        <ar-list :data-table="tableData5" :data-loading="loading"  @handle-refresh="refresh"></ar-list>
        <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :page-sizes="[10, 50, 100, 500]"
      :page-size="psize"
      :current-page.sync="currentPage"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
      </el-card>
    </div>
  </div>

</template>

<script>
import ArList from '@/components/suplier/Ar/ArListCancel'
import Search from '@/components/suplier/Ar/SearchCancel'
import DataInit from '@/mixins/suplier/Ar/DataInit'
import Table from '@/mixins/suplier/Ar/Table'
export default {
  mixins: [DataInit, Table],
  data () {
    return {
      postUrl: '/transferedAr/getTransferedArListTable.do',
      totalStr: 'total', // 服务器返回总数参数名
      dataStr: 'datas' // 服务器返回数据参数名
    }
  },
  components: {
    'ar-list': ArList,
    'search': Search
  },
  mounted () {
    const that = this
    this.getdata(1, 10)
      .then(function (response) {
        console.log(response)
        if (response) {
          that.tableData5 = response.data[that.dataStr]
          that.total = response.data[that.totalStr]
        }
      })
      .catch(function (error) {
        console.log(error)
      })
  },
  methods: {
    // 条件查询
    handleSearch (val) {
      console.log(val)
      let form = val.moneyDate ? val.moneyDate[0].Format('yyyy-MM-dd') : ''
      let to = val.moneyDate ? val.moneyDate[1].Format('yyyy-MM-dd') : ''
      this.param = {
        masterChain: val.masterChainId, // ar单号
        isMasterAr: val.isMasterAr, // ar来源
        companyName: val.companyName, // 付款单位
        checkedStatus: val.status, // 状态
        billBookCurr: val.billBookCurr, // 币别
        invoiceNo: val.invoiceNo, // 发票号
        from: form, // 日期
        to: to,
        transSerialNo: val.transSerialNo // 交易流水号
      }
      console.log('1')
      if (this.total && this.currentPage !== 1) {
        console.log('2')
        this.total = 0 // 分页的当前页数变动会触发 从而获取数据
      } else {
        console.log('3')
        this.getdata(1, 10).then(res => {
          if (res) {
            this.tableData5 = res.data[this.dataStr]
            this.total = res.data[this.totalStr]
          }
        })
      }
    },
    refresh () {
      const that = this
      this.getdata(that.currentPage, that.psize)
        .then(res => {
          if (res) {
            this.tableData5 = res.data[this.dataStr]
            this.total = res.data[this.totalStr]
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}

</script>

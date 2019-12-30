<template scoped>
  <div>
    <!--查询弹出框-->
    <el-dialog title="收支管理查询" v-model="search.visible" size="small">
      <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
  	    <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: left">
              <el-checkbox label="开始日期" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16" style="text-align: center">
              <el-date-picker style="margin-top: 5px; margin-left: 10px;" v-model="search.data.startTime"  type="date" format="yyyy-MM-dd" placeholder="选择开始日期":disabled="!search.useStartTime">
              </el-date-picker>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="供应商名称" v-model="search.useSupplierId" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select filterable style="width: 100%;" v-model="search.data.supplierId" :disabled="!search.useSupplierId">
                <el-option v-for="item in search.supplier_options"
                  :key="item.id"
                  :label="item.name"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-col>
          </el-row>
  	    </el-col>
  	    <el-col :span="12">
  	       <el-row class="bottom20">
  	        <el-col :span="5" style="text-align: left">
  	          <el-checkbox label="结束日期" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
  	        </el-col>
  	        <el-col :span="16" style="text-align: center">
  	            <el-date-picker style="margin-top: 5px; margin-left: 10px;" v-model="search.data.endTime"  type="date" format="yyyy-MM-dd" placeholder="选择结束日期":disabled="!search.useEndTime">
  	           </el-date-picker>
  	        </el-col>
  	       </el-row>
  	    </el-col>
      </el-form>
      <div slot="footer" class="dialog-footer">
      <el-button @click="search.visible = false">取 消</el-button>
      <el-button type="primary" @click="handleSearch()">查询</el-button>
      </div>
    </el-dialog>
  
    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="总划账金额" v-bind:part1="totalBalance" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总未发货" v-bind:part1="totalNoSend" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="在途" v-bind:part1="totalSending" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="已收货" v-bind:part1="totalConfirmed" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款" v-bind:part1="totalRefund" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退货" v-bind:part1="totalBack" back="#1D8CE0"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
      <bc-panel title="查询结果 （总未发货=未发货-退款）" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table  v-loading="loading" :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="datetime" label="日期" >
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商" >
          </el-table-column>
          <el-table-column prop="nosend" label="未发货" >
          </el-table-column>  
          <el-table-column prop="sending" label="在途" >
          </el-table-column>           
          <el-table-column prop="confirmed" label="已收货" >
          </el-table-column> 
          <el-table-column prop="refund" label="退款" >
          </el-table-column> 
          <el-table-column prop="back" label="退货" >
          </el-table-column> 
          <el-table-column prop="balance" label="划账金额" >
          </el-table-column> 
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small" @click="printtf()"><i class="icon-printer icons"></i> 打印</el-button>
            </el-button-group>
          </el-col>
          <el-col :span="16" style="text-align: right">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="table.pageIndex"
              :page-sizes="[25, 50, 75, 100]"
              :page-size="25"
              layout="total, sizes, prev, pager, next, jumper"
              :total="table.total">
            </el-pagination>
          </el-col>
        </el-row>
        </bc-panel>
      </el-col>
    </el-row>
    <div id="pdf-wrap" style="display:none;">
      <el-row :gutter="20" style="margin-bottom: 20px">
         <el-col :span='24' style="text-align: center" >
            收支管理
         </el-col>
      </el-row>
      <el-row :gutter="20" style="margin-bottom: 20px;">
        <el-col :span='12' style="text-align: left; margin-top: 20px;" >
          <div style="position: relative;left: 0px;display: inline-block;">供应商：{{strSupplierName}}</div>
          <div style="position: absolute;left: 450px;display: inline-block;">时间段：{{startTime}}至{{endTime}}</div>
        </el-col>
      </el-row>
      <el-row :gutter="20" >
        <el-col :span='24'>
          <table width="100%" border="1" cellspacing="0" cellpadding="0">
            <tr width="100%" style="border-bottom: none;">
              <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">日期</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 150">供应商</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 100">未发货</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">在途</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">已收货</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">退款</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">退货</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">划账金额</td>
            </tr>
            <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
              <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.datetime}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.nosend}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.sending}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.confirmed}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refund}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.back}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.balance}}</td>
            </tr>
            <tr width="100%" style="border-bottom: none;">
              <td style="padding: 5px;font-size:12px;text-align:center;">合计</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{totalNoSend}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{totalSending}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{totalConfirmed}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{totalRefund}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{totalBack}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{totalBalance}}</td>
            </tr> 
          </table>
        </el-col>
      </el-row>
      <el-row :gutter="20" style="margin-bottom: 20px;position: relative;">
        <el-col :span='12' style="text-align: left; margin-top: 20px;font-size: 13px;" >
          <span style="position: relative;left: 0px;display: inline-block;">时间：{{$filter(new Date(),"",0)}}</span>
          <span style="position: relative;left: 500px;display: inline-block;">制表人：</span>
          <span style="position: absolute;left: 900px;display: inline-block;">审核人：</span>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<style>
  .el-date-editor.el-input{
    width: 100%;
  }
</style>
<script>
module.exports = {
  data: function () {
    return {
    	searchStr: '',
      table: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        list: [],
        selections: []
      },
      loading:false,
      totalNoSend:0,
      totalSending:0,
      totalConfirmed:0,
      totalRefund:0,
      totalBack:0,
      totalBalance:0,
      startTime: '',
      endTime: '',
      strSupplierName: '',                   
      search: {
        visible: false,
        useStartTime: true,
        useEndTime: true,
        useSupplierId: true,
        supplier_options: [],
        data: {
          balanceLessThan: '',
          startTime: '',
          endTime: '',
          supplierId: '',
        }
      }
    }
  },
  methods: {
    handleSelectionChange(val) {
      this.table.selections = val;
    },
    handleSizeChange(val) {
      this.table.pageCount = val;
       this.handleSearch();
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
       this.handleSearch();
    },
    getSearch(key) {
      var me=this;
      me.searchStr = '搜索';

      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime = me.$formatDate(this.search.data.startTime);
        me.searchStr += ' 开始时间： ' + key.startTime;
          if(key.startTime != '') {
            me.searchStr += ',';
          }
      }    
      if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime = me.$formatDate(this.search.data.endTime);
        me.searchStr += ' 结束时间： ' + key.endTime;
          if(key.endTime != '') {
            me.searchStr += '；';
          }
      }
      if(this.search.useSupplierId&&this.search.data.supplierId){
        key.supplierId=this.search.data.supplierId;
        for (var i = 0; i < this.search.supplier_options.length; i++) {
          if (key.supplierId == this.search.supplier_options[i].id) {
            me.searchStr += ' 供应商名称： ' + this.search.supplier_options[i].name + '，';
          }
        }
      }
      return key;
    },
    handleSearch() {
      var key={};
      var me = this;
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      key = me.getSearch(key);
      
      this.$get("admin/shouzhi/many",key).then2(function(response){
        me.table.items = response.data.data;
        me.table.list = response.data.list;
        me.table.total = response.data.totalRow;
        me.totalBalance = response.data.totalBalance;
        me.totalNoSend = response.data.totalNoSend;
        me.totalSending = response.data.totalSending;
        me.totalConfirmed = response.data.totalConfirmed;
        me.totalRefund = response.data.totalRefund;
        me.totalBack = response.data.totalBack;
        me.totalBalance = response.data.totalBalance;
        me.startTime = response.data.startTime;
        me.endTime = response.data.endTime;
        me.strSupplierName = response.data.strSupplierName;
      }).catch(function(err){
        console.log(err);
      });
      me.search.visible = false;
    },
    getAllSupplier(){
      var me = this;
      me.$get("admin/supplier/allSuppliers",{}).then2(function(response){
        me.search.supplier_options = response.data.data;
      }).catch(function(err){
        console.log(err);
      });
    },
    handleExport() {
      var key = this.getSearch({});
      this.$get('admin/shouzhi/export', key).then2(function (response) {
        if (response.data.error > 0) {
          alert(response.errmsg);
        } else {
          window.open(response.data.path);
        }
      }).catch(function(err){
         console.log(err)
      });
    },
    //打印功能
    printtf:function(){
      this.bundprint('pdf-wrap');
    },
  },mounted:function(){
    this.handleSearch();
    this.getAllSupplier();
  }
}
</script>
<style>
	.input-size{
		width: 87%	
	}
</style>

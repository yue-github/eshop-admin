<template scoped>
  <div>
  	<!--查询弹出框-->
    <el-dialog title="查询销售明细" v-model="search.visible" size="small" top="5vh">
	    <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
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

          <el-row class="bottom20">
            <el-col :span="6" >
              <el-checkbox label="开始收货日期" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
             <el-col :span="16">
                <el-date-picker v-model="search.data.startTime"  type="date" placeholder="开始收货日期" :disabled="!search.useStartTime">
                </el-date-picker>
             </el-col>
          </el-row>
	    	</el-col>
	    	
	    	<el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="发票类型" v-model="search.useInvoiceType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="search.data.invoiceType" :disabled="!search.useInvoiceType">
                <el-option label="请选择发票类型" value=""></el-option>
                <el-option label="增值税普通发票" value="value_add"></el-option>
                <el-option label="增值税专用发票" value="general"></el-option>
              </el-select>
            </el-col>
          </el-row>

	    		<el-row class="bottom20">
            <el-col :span="6" >
              <el-checkbox label="结束收货日期" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
               <el-date-picker v-model="search.data.endTime"  type="date" placeholder="结束收货日期" :disabled="!search.useEndTime">
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
      <el-col :span="4"><bc-statbox title="总销售额" v-bind:part1="totalPayable" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总含税成本" v-bind:part1="totalUnitCost" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总未税成本" v-bind:part1="totalUnitCostNoTax" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="微信支付金额" v-bind:part1="totalWeixinpay" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="支付宝支付金额" v-bind:part1="totalAlipay" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="银联支付金额" v-bind:part1="totalUnionpay" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="团购卡支付金额" v-bind:part1="totalCardpay" back="#58B7FF" style='margin-top:20px;'></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="积分兑换金额" v-bind:part1="totalPointpay" back="#58B7FF" style='margin-top:20px;'></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="10">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	{{searchStr}}
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table v-loading="loading" :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商名称">
          </el-table-column>
          <el-table-column label="支付方式" width="100" align="center">
            <el-table-column prop="weixinpay" label="微信(网页、APP)">
            </el-table-column>
            <el-table-column prop="alipay" label="支付宝支付">
            </el-table-column>
            <el-table-column prop="unionpay" label="银联支付">
            </el-table-column>
            <el-table-column prop="cardpay" label="团购卡支付">
            </el-table-column>
            <el-table-column prop="pointpay" label="积分兑换支付">
            </el-table-column>
          </el-table-column>
           <el-table-column prop="payable" label="收入合计" align="center">
          </el-table-column>  
          <el-table-column label="成本" width="175" align="center">
            <el-table-column prop="taxRate" label="税率">
            </el-table-column>
            <el-table-column prop="totalUnitCostNoTax" label="未税">
            </el-table-column>
            <el-table-column prop="totalUnitCost" label="含税">
            </el-table-column>
          </el-table-column>           
          <el-table-column prop="invoiceType" label="备注">
          </el-table-column> 
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="info" size="small"  @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
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
                电商收入成本表（已收货）
             </el-col>
          </el-row>
          <el-row :gutter="20" style="margin-bottom: 20px;">
	      	  <el-col :span='12' style="text-align: left; margin-top: 20px;" >
         		  <div style="position: relative;left: 0px;display: inline-block;">已收货对账时间：{{searchMap.startTime}}至{{searchMap.endTime}}</div>
         		  <div style="position: absolute;left: 950px;display: inline-block;">单位：元</div>
	         </el-col>
          </el-row>
          <el-row :gutter="20" >
            <el-col :span='24'>
              <table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 40px;" rowspan="2">序号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 500px;" rowspan="2">供应商名称</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 500px;" colspan="5">支付方式</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px;" rowspan="2">收入合计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 500px;" colspan="3">成本</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px;border-right: 1px solid #9a9a9a;" rowspan="2">备注</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">微信(网页、APP)</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">支付宝支付</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">银联支付</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">团购卡支付</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">积分兑换支付</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">税率</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">未税</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 160px">含税</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.weixinpay}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.alipay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unionpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.cardpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.pointpay}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.payable}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRate}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalUnitCostNoTax}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalUnitCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.invoiceType}}</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;" colspan="2">合计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalWeixinpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalAlipay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalUnionpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalCardpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalPointpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{totalUnitCostNoTax}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;border-bottom: 1px solid #9a9a9a;">{{totalUnitCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;border-bottom: 1px solid #9a9a9a;">&nbsp;</td>
                </tr> 
                <tr width="100%" style="border-bottom: none;">
                  <td colspan="10" style="padding: 5px;border-left:0px;border-bottom:0px;border-right:0px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                <tr width="100%" style="border: none;">
                  <td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">时间：{{$filter(new Date(),"",0)}}</td>
                  <td colspan="4"  style="padding: 5px;border: none;font-size:12px;text-align:left;">制表人：</td>
                  <td colspan="3"  style="padding: 5px;border: none;font-size:12px;text-align:left;">审核人：</td>
                </tr>
              </table>
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
        selections: [],
      },
      loading:false,
      totalPayable:0,    
      totalUnitCost:0,    
      totalUnitCostNoTax:0,    
      totalWeixinpay:0,    
      totalAlipay:0,    
      totalUnionpay:0,
      totalPointpay:0,
      search: {
      	visible: false,
        useOrder_code: true,
        useSupplierId: true,
        useInvoiceType: true,
        useStartTime: true,
        useEndTime: true,
        supplier_options: [],
        
        data: {
          supplierId: '',
          invoiceType: '',
          startTime: '',
          endTime: '',
        }
      },
      searchMap: {
        supplierName: '',
        invoiceType: '',
        startTime: '',
        endTime: '',
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
    handleCreate() {
      
    },
    handleEdit(id, row) {
      
    },
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {

    },
    getInvoiceType(invoiceType) {
      if (invoiceType == 'general') {
        return '增值税普通发票';
      } else if (invoiceType == 'value_add') {
        return '增值税专用发票';
      } else {
        return '';
      }
    },
    getSearchKey(key) {
      var me = this;
      me.searchStr = "";
      if(this.search.useSupplierId&&this.search.data.supplierId){
        key.supplierId=this.search.data.supplierId;
        for (var i = 0; i < this.search.supplier_options.length; i++) {
          if (key.supplierId == this.search.supplier_options[i].id) {
            me.search.strSupplierName = this.search.supplier_options[i].name;
            me.searchStr += ' 供应商名称： ' + this.search.supplier_options[i].name + '，';
          }
        }
      }
      if(this.search.useInvoiceType&&this.search.data.invoiceType){
        key.invoiceType=this.search.data.invoiceType;
        if (key.invoiceType) {
          me.searchStr += '发票类型：' + me.getInvoiceType(key.invoiceType) + '，';
        }
      }
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime=this.$formatDate(this.search.data.startTime);
        if (key.startTime) {
          me.searchStr += '开始收货日期：' + key.startTime + '，';
          me.searchMap.startTime = key.startTime;
        }
      }
      if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        if (key.endTime) {
          me.searchStr += '结束收货日期：' + key.endTime + '，';
          me.searchMap.endTime = key.endTime;
        }
      }

      return key;
    },
    handleSearch() {
      var key={};
      var me=this;
      me.searchStr = '搜索';
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      key = this.getSearchKey(key);

      this.$get("admin/saleCost/many",key)
          .then2(function(response){
          	  for(var i=0;i<response.data.data.length;i++){
                var item = response.data.data[i];
                item.taxRate = item.taxRate + "%";
	            }
              for(var i=0;i<response.data.list.length;i++){
                var item = response.data.list[i];
                item.taxRate = item.taxRate + "%";
              }
              me.table.items=response.data.data;
	            me.table.list=response.data.list;
	            me.table.total=response.data.totalRow;
	            me.totalPayable=response.data.totalPayable;
	            me.totalUnitCost=response.data.totalUnitCost;
	            me.totalUnitCostNoTax=response.data.totalUnitCostNoTax;
	            me.totalWeixinpay=response.data.totalWeixinpay;
	            me.totalAlipay=response.data.totalAlipay;
              me.totalUnionpay=response.data.totalUnionpay;
              me.totalCardpay=response.data.totalCardpay;
              me.totalPointpay=response.data.totalPointpay;
              me.searchMap.supplierName = response.data.supplierName;
	            me.search.visible = false;
              console.log(1);
              console.log(response.data);
      }).catch(function(err){
       console.log(err);
      });
    },
    //导出Excel表
    handleExport(){
      var key = {};
      key = this.getSearchKey(key);
      this.$get("admin/saleCost/export", key)
          .then2(function(response){
            if (response.data.error > 0) {
                return false;
            } else {
                 window.open(response.data.path);
            }        
      }).catch(function(err){
       console.log(err);
      });
    },
    //打印功能
    printtf:function(){
      this.bundprint('pdf-wrap');
    },
    getAllSupplier(){
      var me = this;
      me.$get("admin/supplier/allSuppliers",{}).then2(function(response){
        me.search.supplier_options = response.data.data;
      }).catch(function(err){
        console.log(err);
      });
    },
    initSearchStr() {
      this.searchStr += '开始收货日期：' + this.$getMonthFirstDay() + '，';
      this.searchStr += '结束收货日期：' + this.$getMonthLastDay() + '，';
      this.searchMap.startTime = this.$getMonthFirstDay();
      this.searchMap.endTime = this.$getMonthLastDay();
    },
  },mounted:function(){
    this.handleSearch();
    this.getAllSupplier();
    this.initSearchStr();
  }
}
</script>
<style>
	.input-size{
		width: 87%	
	}
</style>
<template scoped>
  <div>
  	<!--高级查询弹出框-->
    <el-dialog title="查询供应商对账明细" v-model="search.visible" size="small">
	    <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="供应商 " v-model="search.useSupplierId" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select filterable style="width: 100%;" v-model="search.data.supplierId" placeholder="请选择供应商" :disabled="!search.useSupplierId">
                <el-option v-for="item in search.supplier_options"
                  :key="item.id"
                  :label="item.name"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="订单状态" v-model="search.useStatus" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="search.data.status" placeholder="请选择订单状态" :disabled="!search.useStatus">
                <el-option label="已支付" value="1"></el-option>
                <el-option label="已发货" value="2"></el-option>
                <el-option label="已收货" value="3"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="结束时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16" style="text-align: center">
              <el-date-picker v-model="search.data.endTime"  type="date" placeholder="选择发货日期" :disabled="!search.useEndTime">
              </el-date-picker>
            </el-col>
          </el-row> 
		    </el-col>
		    <el-col :span="12">
		    	<el-input placeholder="请输入供应商名称" v-model="search.data.supplierName" :disabled="!search.useSupplierName" class="bottom20 input-size">
            <el-checkbox label="供应商名称" slot="prepend" v-model="search.useSupplierName"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>  
            <el-col :span="16" style="text-align: center">
              <el-date-picker v-model="search.data.startTime" type="date" placeholder="选择发货日期" :disabled="!search.useStartTime">
              </el-date-picker>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="支付方式" v-model="search.usePayType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" multiple v-model="search.data.payType" :disabled="!search.usePayType">
                <el-option label="微信支付" value="1"></el-option>
                <el-option label="支付宝支付" value="2"></el-option>
                <el-option label="银联支付" value="3"></el-option>
                <el-option label="团购卡支付" value="5"></el-option>
                <el-option label="积分兑换支付" value="6"></el-option>
              </el-select>
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
      <el-col :span="4"><bc-statbox title="总销售金额" v-bind:part1="totalSale" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="产品总数量" v-bind:part1="totalUnitOrdered" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款总数量" v-bind:part1="totalRefundAmount" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款总金额" v-bind:part1="totalRefundCash" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总成本" v-bind:part1="totalSaleCost" back="#58B7FF"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商" width="200">
          </el-table-column>
          <el-table-column prop="payTime" label="支付日期" width="200">
          </el-table-column> 
          <el-table-column prop="sendOutTime" label="发货日期" width="200">
          </el-table-column> 
          <el-table-column prop="order_code" label="订单号" width="200">
          </el-table-column>  
          <el-table-column prop="product_name" label="产品名称及规格" width="300">
          </el-table-column>       
          <el-table-column prop="unitOrdered" label="发货数量" width="100">
          </el-table-column>
          <el-table-column prop="totalProductCost" label="产品成本小计" width="150">
          </el-table-column>  
          <el-table-column prop="deliveryPrice" label="快递费用" width="100">
          </el-table-column> 
          <el-table-column prop="totalCost" label="总成本" width="100">
          </el-table-column> 
          <el-table-column prop="totalActualProductPrice" label="产品销售金额小计" width="150">
          </el-table-column> 
          <el-table-column prop="refundCash" label="退货总金额" width="130">
          </el-table-column> 
          <el-table-column prop="totalSale" label="销售总额" width="100">
          </el-table-column> 
          <el-table-column prop="payType" label="支付方式" width="100">
          </el-table-column>
          <el-table-column prop="source" label="平台类型" width="100">
          </el-table-column>
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
                <i class="icon-minus icons"></i> 禁用
              </el-button> 
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
                 供应商对账明细表
             </el-col>
          </el-row>
          <el-row :gutter="20" >
            <el-col :span='24'>
              <table width="100%" border="1" cellspacing="0" cellpadding="0">
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left;">供货单位：{{spName}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">收货单位：广东通驿高速公路服务区有限公司</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left;">联系人：{{spContactPerson}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">联系人：陈家忠</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left;">电话：{{spPhone}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">电话：020-22353742</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left;">结款方式：{{spAccountPeriod}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">供应商</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150">支付日期</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150">发货日期</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">订单号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px">产品名称及规格</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 25px">发货数量</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">产品成本小计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 25px">快递费用</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">总成本</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px;">产品销售金额小计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">退货总额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">销售总额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">支付方式</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">平台类型</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payTime}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.sendOutTime}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.order_code}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.product_name}}({{item.selectProterties}})</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
                 	 <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalProductCost}}</td>
                 	 <td style="padding: 5px;font-size:12px;text-align:center;">{{item.deliveryPrice}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalActualProductPrice}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalSale}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payType}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.source}}</td>
                </tr>
                <tr width="100%" >
                  <td colspan="15" style="padding: 5px;font-size:12px;text-align:left;">截止{{$filter(search.data.endTime,"y.m.d",0)}}，累计发货 {{totalUnitOrdered}}  单，累计退货 {{totalRefundAmount}}  单， 销售总成本{{ totalSaleCost}}  元，销售总额{{ totalSale}}   元，；其中微信支付 {{weixin}} 元(微信App：{{weixinApp}} 元，微信网页：{{weixinPc}} 元)，支付宝支付：{{alipay}}  元，银联支付：{{unionpay}} 元</td>
                </tr> 
                <tr width="100%" >
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}累计应收（付）货款：{{ totalSaleCost}}  元</td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}应开增值税普通（专用）发票累计：{{ totalSaleCost}}   元</td>
                </tr>
                <tr width="100%">
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left; height: 50px;">甲方确认：</td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left; height: 50px;">乙方确认：</td>
                </tr>
                 <tr width="100%" >
                   <td colspan="7" style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
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
      totalSaleCost:0,
      totalRefundAmount:0,
      totalRefundCash:0,
      totalUnitOrdered:0,
      totalSale:0,
      weixin: 0,
      weixinApp: 0,
      weixinPc: 0,
      alipay: 0,
      unionpay: 0,
      spName: '',
      spContactPerson: '',
      spPhone: '',
      spAccountPeriod: '',
      search: {
      	visible: false,
        useStartTime: true,
        useEndTime: true,
        useSupplierName: true,
        useSupplierId:true,
        useStatus:true,
        usePayType:true,
        data: {
          startTime: '',
          endTime: '',
          supplierName: '',
          supplierId: '',
          status: '2',
          payType: [],
        },
        supplier_options:[],
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
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {

    },
    getStatus(status) {
      if (status == 1) {
        return '已支付';
      } else if (status == 2) {
        return '已发货';
      } else if (status == 3) {
        return '已收货';
      } else {
        return '';
      }
    },
    getSearchKey(key) {
      var me = this;
      me.searchStr = "";
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime=this.$formatDate(this.search.data.startTime);
        me.searchStr += ' 开始时间：' + key.startTime;
        if(key.startTime != '') {
          me.searchStr += '；';
        }
      }
      if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        me.searchStr += ' 结束时间： ' + key.endTime;
        if(key.endTime != '') {
          me.searchStr += '；';
        }
      }
      if(this.search.useSupplierName&&this.search.data.supplierName){
        key.supplierName=this.search.data.supplierName;
        me.searchStr += ' 供应商名称： ' + key.supplierName;
        if(key.supplierName != '') {
          me.searchStr += '；';
        }
      }
      if(this.search.useSupplierId&&this.search.data.supplierId){
        key.supplierId=this.search.data.supplierId;
        for (var i = 0; i < me.search.supplier_options.length; i++) {
          var item = me.search.supplier_options[i];
          if (item.id == key.supplierId) {
            me.searchStr += ' 供应商: ' + item.name + ';';
            break;
          }
        }
      }
      if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
        me.searchStr += ' 订单状态： ' + this.getStatus(key.status);
        if(key.status != '') {
          me.searchStr += '；';
        }
      }
      if(this.search.usePayType&&this.search.data.payType.length > 0){
        key.payType = JSON.stringify(this.search.data.payType);
        me.searchStr += ' 支付方式: ' + this.$getOrderPayTypes(this.search.data.payType);
        if(key.searchStr!=""){
          me.searchStr += "；"
        }
      }
      return key;
    },
    handleSearch() {
        var me=this;
        var key={};
        me.searchStr = '搜索';
        key.length=this.table.pageCount ;
        key.offset= (this.table.pageIndex-1)*this.table.pageCount;
        key = this.getSearchKey(key);

        this.$get("admin/supplier/supplierOrderList",key).then2(function(response){
          for (var i = 0; i < response.data.data.length; i++) {
            var item = response.data.data[i];
            item.selectProterties = (item.selectProterties == null) ? '' : item.selectProterties;
            item.product_name =  item.product_name + " " + item.selectProterties;
            item.invoiceType = me.getInvoiceType(item.invoiceType);
            item.payType = me.$getOrderPayType(item.payType);
            item.source = me.$getOrderSource(item.source);
          }
          for (var i = 0; i < response.data.list.length; i++) {
            var item = response.data.list[i];
            item.selectProterties = (item.selectProterties == null) ? '' : item.selectProterties;
            item.product_name =  item.product_name + " " + item.selectProterties;
            item.invoiceType = me.getInvoiceType(item.invoiceType);
            item.payType = me.$getOrderPayType(item.payType);
            item.source = me.$getOrderSource(item.source);
          }
          me.table.list=response.data.list;
          me.table.items=response.data.data;
          me.table.total=response.data.totalRow;
          me.totalSaleCost=response.data.totalSaleCost;
          me.totalRefundAmount=response.data.totalRefundAmount;
          me.totalRefundCash=response.data.totalRefundCash;
          me.totalUnitOrdered=response.data.totalUnitOrdered;
          me.totalSale=response.data.totalSale;
          me.weixin=response.data.weixin;
          me.weixinPc=response.data.weixinPc;
          me.weixinApp=response.data.weixinApp;
          me.alipay=response.data.alipay;
          me.unionpay=response.data.unionpay;
          me.search.visible=false;
          me.spName=response.data.spName;
          me.spContactPerson=response.data.spContactPerson;
          me.spPhone=response.data.spPhone;
          me.spAccountPeriod=response.data.spAccountPeriod;
        }).catch(function(err){
          console.log(err);
        });
    },
    getInvoiceType(invoiceType) {
      if (invoiceType == 'general') {
        return '普通发票';
      } else if (invoiceType == 'value_add') {
        return '增值税发票';
      } else {
        return '';
      }
    },
    handleExport() {
      var key = this.getSearchKey({});
      this.$get('admin/supplier/exportSupplierOrderList', key).then2(function (response) {
        if (response.data.error > 0) {
          
        } else {
          window.open(response.data.path);
        }
      }).catch(function(err){
        console.log(err)
      });
    },
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
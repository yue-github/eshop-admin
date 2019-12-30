<template scoped>
  <div>
  	<!--查询弹出框-->
    <el-dialog title="查询供应商对账汇总信息" v-model="search.visible" size="small">
	    <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="供应商 " v-model="search.useSupplierId" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select filterable style="width: 100%;" v-model="search.data.supplier_id" placeholder="请选择供应商" :disabled="!search.useSupplierId">
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
              <el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>  
            <el-col :span="16" style="text-align: center">
              <el-date-picker v-model="search.data.startTime" type="date" placeholder="选择发货日期" :disabled="!search.useStartTime">
              </el-date-picker>
            </el-col>
          </el-row>
		    </el-col>
		    <el-col :span="12">
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
	    </el-form>
	    <div slot="footer" class="dialog-footer">
		    <el-button @click="search.visible = false">取 消</el-button>
		    <el-button type="primary" @click="allSearch()">查询</el-button>
	    </div>
  	</el-dialog>
  	
    <el-row :gutter="24" class="bottom20">
      <el-col :span="4"><bc-statbox title="总销售金额" v-bind:part1="totalSalable" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="产品总数量" v-bind:part1="totalSendAmount" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款总数量 " v-bind:part1="totalBackAmount" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款总金额" v-bind:part1="totalRefund" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总成本" v-bind:part1="totalCost" back="#58B7FF  "></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
  		</bc-panel>
      </el-col>
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible=true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="sendOutTime" label="发货日期">
          </el-table-column>  
          <el-table-column prop="supplier_name" label="供应商">
          </el-table-column>
          <el-table-column prop="tradeType" label="交易类型">
          </el-table-column>          
          <el-table-column prop="unitOrdered" label="发货总数量">
          </el-table-column> 
          <el-table-column prop="totalProductCost" label="产品总成本">
          </el-table-column> 
          <el-table-column prop="totalActualDeliveryCharge" label="快递总费用">
          </el-table-column> 
          <el-table-column prop="totalCost" label="总成本">
          </el-table-column> 
          <el-table-column prop="totalPrice" label="产品总金额">
          </el-table-column> 
          <el-table-column prop="backAmount" label="退货总数量">
          </el-table-column> 
          <el-table-column prop="totalRefund" label="退货总金额">
          </el-table-column> 
          <el-table-column prop="totalSalable" label="总销售额">
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
                  供应商对账汇总表（{{spStatus}}）
             </el-col>
          </el-row>
          <el-row :gutter="20" >
            <el-col :span='24'>
              <table width="100%" border="1" cellspacing="0" cellpadding="0">
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">供货单位：{{spName}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">收货单位：广东通驿高速公路服务区有限公司</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">联系人：{{spContactPerson}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">联系人：陈家忠</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">电话：{{spPhone}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">电话：020-22353742</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">结款方式：{{spAccountPeriod}}</td>
                   <td colspan="8" style="padding: 5px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
                   <td colspan="2" style="padding: 5px;font-size:12px;text-align:center;width: 60px">发货日期</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">供应商</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">交易类型</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">发货总数量</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">产品总成本</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">快递总费用</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">总成本</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">产品总金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">退货总数量</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">退货总金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">总销售额</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
                   <td colspan="2" style="padding: 5px;font-size:12px;text-align:left;">{{item.sendOutTime}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeType}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalProductCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalActualDeliveryCharge}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalCost}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPrice}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.backAmount}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalRefund}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalSalable}}</td>
                </tr>
                <tr>
                  <td colspan="14" style="padding: 5px;font-size:12px;text-align:left;">截止{{endTime}}，累计发货  {{totalSendAmount}}  单, 累计退货 {{totalBackAmount}} 单，销售总成本 {{totalCost}} 元，销售总额  {{totalSalable}} 元，其中微信支付 {{totalWeiXin}} 元(微信App：{{totalWxApp}} 元，微信网页： {{totalWxPc}}元)，
  支付宝支付： {{totalAlipay}} 元，银联支付：{{totalUnionPay}} 元</td>
                </tr> 
                <tr width="100%" >
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">{{startTime}}-{{endTime}}累计应收（付）货款： {{totalCost}} 元 </td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left;">{{startTime}}-{{endTime}}应开增值税普通（专用）发票累计： {{totalCost}}  元</td>
                </tr>
                <!-- <tr width="100%" >
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">上月货款金额：</td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left;">{{startTime}}-{{endTime}}应（收）付快递成本累计：0.0</td>
                </tr>
                <tr width="100%" >
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">上月开票金额：      </td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left;">上月快递成本总金额：  </td>
                </tr> -->

                <tr width="100%">
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">甲方确认：</td>
                   <td colspan="8"  style="padding: 5px;font-size:12px;text-align:left;">乙方确认：</td>
                </tr>
                <tr width="100%" >
                   <td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
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
        selections: [],
      },
      spStatus: '',
      spName: '',
      spContactPerson:'',
      spPhone:'',
      spAccountPeriod:'',
      startTime:'',
      endTime:'',
      supplier:{},
      totalSalable:0,
      totalPrice:0,
      totalSendAmount:0,
      totalBackAmount:0,
      totalRefund:0,
      totalCost:0,
      totalWeiXin:0,
      totalWxApp:0,
      totalWxPc:0,
      totalAlipay:0,
      totalUnionPay:0,
      items:[],
      search: {
      	visible: false,
		    useStartTime:true,
		    useEndTime:true,
        useSupplierId:true,
        useStatus:true,
        useSupplierName: true,
        data: {
          startTime:'',
          endTime:'',
          supplierName: '',
          supplier_id:'',
          status: '2',
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
      this.allSearch();
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
      this.allSearch();
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
    getSearchSection() {
    	var me = this;
      var key={};
      me.searchStr = "搜索";
      key.length=this.table.pageCount;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      if (this.search.useSupplierId && this.search.data.supplier_id) {
        key.supplier_id = this.search.data.supplier_id;
        for (var i = 0; i < this.search.supplier_options.length; i++) {
          var item = this.search.supplier_options[i];
          if (item.id == key.supplier_id) {
            me.searchStr += ' 供应商： ' + item.name +"；";
            break;
          }
        }
      }
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime= this.$formatDate(this.search.data.startTime);
        me.searchStr += ' 开始时间： ' + key.startTime ;
        if(key.startTime!=""){
        	 me.searchStr+=";"
        }
      }
      if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        me.searchStr += ' 结束时间： ' + key.endTime ;
        if(key.endTime!=""){
        	 me.searchStr+=";"
        }
      }
      if(this.search.useSupplierName&&this.search.data.supplierName){
        key.supplierName=this.search.data.supplierName;
        me.searchStr += 'bb' + key.supplierName + '",';
      }
      if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
        me.searchStr += ' 订单状态： ' + this.getStatus(key.status) + '；';
      }
      var status = this.search.data.status;
      if (status == 1) {
        this.spStatus = '已支付';
      } else if (status == 2) {
        this.spStatus = '已发货';
      } else if (status == 3) {
        this.spStatus = '已收货';
      }
      return key;
    },
    handleSearch() {
      var me = this;
      me.searchStr = '搜索';
      var key = me.getSearchSection();
      
      this.$post("admin/supplier/supplierOrderSummary",key).then2(function(response){
        var items = response.data.data;
        var list = response.data.list;
        for (var i = 0; i < items.length; i++) {
          items[i].totalCost = items[i].totalProductCost + items[i].totalActualDeliveryCharge;
        }
        for (var i = 0; i < list.length; i++) {
          list[i].totalCost = list[i].totalProductCost + list[i].totalActualDeliveryCharge;
        }
        me.table.items=items;
        me.table.list=list;
        me.table.total=response.data.totalRow;
        me.totalSalable = response.data.totalSalable;
        me.totalPrice = response.data.totalPrice;
        me.totalSendAmount = response.data.totalSendAmount;
        me.totalBackAmount = response.data.totalBackAmount;
        me.totalRefund = response.data.totalRefund;
        me.totalCost = response.data.totalCost;
        me.totalWeiXin = response.data.totalWeiXin;
        me.totalWxApp = response.data.totalWxApp;
        me.totalWxPc = response.data.totalWxPc;
        me.totalAlipay = response.data.totalAlipay;
        me.totalUnionPay = response.data.totalUnionPay;
        me.startTime = response.data.startTime;
        me.endTime = response.data.endTime;
        me.spName = response.data.spName;
        me.spContactPerson = response.data.spContactPerson;
        me.spPhone = response.data.spPhone;
        me.spAccountPeriod = response.data.spAccountPeriod;
        me.search.visible=false;
      }).catch(function(err){
        console.log(err);
      });
    },
    handleExport(){
      var key = this.getSearchSection();
      this.$post("admin/supplier/exportSupplierOrderSummary",key).then2(function(response){
        if (response.data.error > 0) {
          return false;
        } else {
          window.open(response.data.path);
        }
      }).catch(function(err){
        console.log(err);
      });
    },
    getAllSupplier(){
      var me = this;
      me.$get("admin/supplier/allSuppliers",{}).then2(function(response){
        me.search.supplier_options = response.data.data;
        console.log(me.search.supplier_options);
      }).catch(function(err){
        console.log(err);
      });
    },
    printtf:function(){      
      this.bundprint('pdf-wrap');
    },
    allSearch(){
    	 this.handleSearch();
    }
  },mounted:function(){
    this.allSearch();
    this.getAllSupplier();
  }
}

</script>
<style>
	.input-size{
		width: 87%	
	}
</style>
<template scoped>
  <div>
  	<!--高级查询弹出框-->
    <el-dialog title="查询销售汇总" v-model="search.visible" size="small">
	    <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="供应商 " v-model="search.useSupplierId" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select filterable style="width: 100%;margin-left: 10px;" v-model="search.data.supplierId" placeholder="请选择供应商" :disabled="!search.useSupplierId">
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
              <el-date-picker v-model="search.data.startTime" type="date" placeholder="选择开始日期" :disabled="!search.useStartTime" style="margin-left: 10px;">
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
              <el-select style="width: 100%;margin-left: 10px;" v-model="search.data.status" placeholder="请选择订单状态" :disabled="!search.useStatus">
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
              <el-date-picker v-model="search.data.endTime"  type="date" placeholder="选择结束日期" :disabled="!search.useEndTime" style="margin-left: 10px;width: 200px">
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
      <el-col :span="3"><bc-statbox title="总销售金额" v-bind:part1="totalPayable" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="总退款金额" v-bind:part1="totalRefundCash" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="总优惠金额" v-bind:part1="totalCouponDiscount" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="总交易次数" v-bind:part1="totalUnitOrdered" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="总退货次数" v-bind:part1="totalRefundAmount" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="支付宝金额" v-bind:part1="totalAlipay" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="微信支付金额" v-bind:part1="totalWeixinpay" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="3"><bc-statbox title="银联支付金额" v-bind:part1="totalUnionpay" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="3" style="margin-top:15px;"><bc-statbox title="余额支付金额" v-bind:part1="totalWalletpay" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="3" style="margin-top:15px;"><bc-statbox title="团购卡支付金额" v-bind:part1="totalCardpay" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="3" style="margin-top:15px;"><bc-statbox title="积分兑换金额" v-bind:part1="totalPointpay" back="#13CE66"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商">
          </el-table-column> 
          <el-table-column prop="taxRate" label="税率">
          </el-table-column>
          <el-table-column prop="payable" label="销售金额">
          </el-table-column>
          <el-table-column prop="couponDiscount" label="优惠金额">
          </el-table-column>  
          <el-table-column prop="unitOrdered" label="交易次数">
          </el-table-column>           
          <el-table-column prop="refundCash" label="退货金额">
          </el-table-column> 
          <el-table-column prop="refundAmount" label="退货次数">
          </el-table-column> 
          <el-table-column prop="alipay" label="支付宝支付金额">
          </el-table-column> 
          <el-table-column prop="weixinpay" label="微信支付金额">
          </el-table-column> 
          <el-table-column prop="unionpay" label="银联支付金额">
          </el-table-column>
          <el-table-column prop="walletpay" label="余额支付金额">
          </el-table-column>
          <el-table-column prop="pointpay" label="积分兑换金额">
          </el-table-column>
          <el-table-column prop="status" label="订单状态">
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
                	销售汇总
             </el-col>
          </el-row>
          <el-row :gutter="20" style="margin-bottom: 20px;">
	      	  <el-col :span='12' style="text-align: left; margin-top: 20px;" >
	           		  <div style="position: relative;left: 0px;display: inline-block;">供应商：{{search.strSupplierName}}</div>
	           		  <div style="position: absolute;left: 400px;display: inline-block;">时间段：{{search.strStartTime}}至{{search.strEndTime}}</div>
	           		  <div style="position: absolute;left: 700px;display: inline-block;">订单状态：{{search.strStatus}}</div>
	           		  <div style="position: absolute;left: 950px;display: inline-block;">单位：元</div>
	         	</el-col>
	        </el-row>
          <el-row :gutter="20" >
            <el-col :span='24'>
              <table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 40px;">序号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px;">供应商</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">税率</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">销售金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">优惠金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">交易次数</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">退货金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">退货次数</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">支付宝支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">微信支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">银联支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">余额支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">团购卡支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;">积分兑换金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;border-right: 1px solid #9a9a9a;">订单状态</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRate}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.payable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.couponDiscount}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundAmount}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.alipay}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.weixinpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unionpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.walletpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.cardpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.pointpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.status}}</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">合计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px"></td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">{{totalCouponDiscount}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalUnitOrdered}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalRefundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalRefundAmount}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalAlipay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">{{totalWeixinpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalUnionpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalWalletpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px;border-bottom: 1px solid #9a9a9a;">{{totalCardpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px;border-bottom: 1px solid #9a9a9a;">{{totalPointpay}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px;border-bottom: 1px solid #9a9a9a;border-right: 1px solid #9a9a9a;">&nbsp;</td>
                </tr> 
                <tr width="100%" style="border-bottom: none;">
                  <td colspan="12" style="padding: 5px;border-left:0px;border-bottom:0px;border-right:0px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                <tr width="100%" style="border: none;">
                   <td colspan="6" style="padding: 5px;border: none;font-size:12px;text-align:left;">时间：{{$filter(new Date(),"",0)}}</td>
                   <td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">制表人：</td>
                   <td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">审核人：</td>
                </tr>
              </table>
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
      statusMap: {
					1: '已支付',
					2: '已发货',
					3: '已收货',
			},
      totalPayable:0,
      totalRefundCash:0,
      totalCouponDiscount:0,
      totalWeixinpay:0,
      totalAlipay:0,
      totalRefundCash:0,
      totalWeixinPrice:0,
      totalUnionpay:0,
      totalWalletpay:0,
      totalCardpay:0,
      totalPointpay:0,
      totalUnitOrdered:0,
      totalRefundAmount:0,
      search: {
      	visible: false,
        useStartTime: true,
        useEndTime: true,
        useSupplierName: true,
        useSupplierId:true,
        useStatus:true,
        supplier_options:[],
        data: {
          startTime: '',
          endTime: '',
          supplierName: '',
          supplierId: '',
          status: '2',
        },
        strStartTime:'',
        strEndTime:'',
        strSupplierName:'',
        strStatus:'',
      },
    }
  },
  methods: {
    handleSelectionChange(val) {
      this.table.selections = val;
      this.allSearch();
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
    getSearchKey(key) {
      var me = this;
      me.searchStr = "搜索";
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime=this.$formatDate(this.search.data.startTime);
        me.searchStr += ' 开始时间： ' + key.startTime ;
        if(key.startTime!=""){
        	me.searchStr +=";";
        }
      }
      if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        me.searchStr += ' 结束时间： ' + key.endTime ;
        if(key.endTime!=""){
        	me.searchStr+=";";
        }
      }
      if(this.search.useSupplierId&&this.search.data.supplierId){
        key.supplierId=this.search.data.supplierId;
        for (var i = 0; i < this.search.supplier_options.length; i++) {
          if (key.supplierId == this.search.supplier_options[i].id) {
            me.search.strSupplierName = this.search.supplier_options[i].name;
            me.searchStr += ' 供应商名称： ' + this.search.supplier_options[i].name;
          }
        }
        if(key.supplierId!=""){
        	 me.searchStr+=";";
        }
      }
      if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
        me.search.strStatus = me.statusMap[key.status];
        me.searchStr += ' 订单状态： ' + this.statusMap[key.status];
        if(key.status!=""){
        	me.searchStr+=";";
        }
      }
      return key;
    },
    handleSearch() {
        var me=this;
        var key={};
        me.searchStr = '搜索';
        key.length=this.table.pageCount ;
        key.offset = (this.table.pageIndex-1)*this.table.pageCount;
	      key = this.getSearchKey(key);

        this.$post("admin/order/orderSummarys",key).then2(function(response){
          for(var i = 0; i < response.data.data.length; i++) {
            var item = response.data.data[i];
						item.status = me.statusMap[me.search.data.status];
						item.taxRate = item.taxRate+"%";
					}
          for(var i = 0; i < response.data.list.length; i++) {
            var item = response.data.list[i];
            item.status = me.statusMap[me.search.data.status];
            item.taxRate = item.taxRate+"%";
          }
          me.table.items = response.data.data;
          me.table.list = response.data.list;
          me.table.total = response.data.totalRow;
          me.totalPayable = response.data.totalPayable;
          me.totalRefundCash = response.data.totalRefundCash;
          me.totalCouponDiscount = response.data.totalCouponDiscount;
          me.totalWeixinpay = response.data.totalWeixinpay;
          me.totalAlipay = response.data.totalAlipay;
          me.totalUnionpay = response.data.totalUnionpay;
          me.totalWalletpay = response.data.totalWalletpay;
          me.totalCardpay = response.data.totalCardpay;
          me.totalPointpay = response.data.totalPointpay;
          me.totalUnitOrdered = response.data.totalUnitOrdered;
          me.totalRefundAmount = response.data.totalRefundAmount;
          me.search.strStartTime = response.data.startTime;
          me.search.strEndTime = response.data.endTime;
          me.search.visible=false;
        }).catch(function(err){
          console.log(err);
        });
    },
    handleExport() {
    	var key = this.getSearchKey({});
	    
      this.$post('admin/order/exportSalesSummary',key).then2(function (response) {
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
        console.log(me.search.supplier_options);
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
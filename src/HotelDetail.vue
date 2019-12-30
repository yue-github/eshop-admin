<template scoped>
  <div>
    <!--查询弹出框-->
    <el-dialog title="查询酒店对账明细" v-model="search.visible" size="small" top="5vh">
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
            <el-col :span="5" >
              <el-checkbox label="开始日期" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
             <el-col :span="16">
                <el-date-picker v-model="search.data.startTime"  type="date" placeholder="开始日期" :disabled="!search.useStartTime">
                </el-date-picker>
             </el-col>
          </el-row>
        </el-col>
        
        <el-col :span="12">
          <el-input  v-model="search.data.orderCode" :disabled="!search.useOrderCode" class="bottom20 input-size">
            <el-checkbox label="订单号" slot="prepend" v-model="search.useOrderCode"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="支付方式" v-model="search.usePayType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" multiple v-model="search.data.payType" :disabled="!search.usePayType">
                <el-option label="请选择支付方式" value=""></el-option>
                <el-option label="微信支付" value="1"></el-option>
                <el-option label="支付宝支付" value="2"></el-option>
                <el-option label="银联支付" value="3"></el-option>
                <el-option label="团购卡支付" value="5"></el-option>
                <el-option label="积分兑换支付" value="6"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="结束日期" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
               <el-date-picker v-model="search.data.endTime"  type="date" placeholder="支付结束日期" :disabled="!search.useEndTime">
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
      <el-col :span="4"><bc-statbox title="总销售额" v-bind:part1="totalSale" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总退款金额" v-bind:part1="totalRefundCash" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总佣金" v-bind:part1="totalCommissionPayable" back="#1D8CE0"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="10">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
        <span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
        <el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table v-loading="loading" :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商" width="200">
          </el-table-column> 
          <el-table-column prop="payTime" label="支付日期" width="200">
          </el-table-column> 
          <el-table-column prop="payType" label="支付方式" width="100">
          </el-table-column> 
          <el-table-column prop="sendOutTime" label="发货日期" width="100">
          </el-table-column> 
          <el-table-column prop="order_code" label="订单号" width="200">
          </el-table-column>
          <el-table-column prop="tradeType" label="交易类型" width="100">
          </el-table-column>
          <el-table-column prop="expressCode" label="快递单号" width="200">
          </el-table-column>  
          <el-table-column prop="logisticsName" label="快递名称" width="175">
          </el-table-column>
          <el-table-column prop="product_name" label="产品名称及规格" width="200">
          </el-table-column>
          <el-table-column prop="unit" label="单位" width="200">
          </el-table-column>  
          <el-table-column prop="unitOrdered" label="发货数量" width="110"> 
          </el-table-column> 
          <el-table-column prop="totalSale" label="产品代售金额小计" width="110"> 
          </el-table-column>
          <el-table-column prop="refundCash" label="退款总金额" width="110"> 
          </el-table-column>
          <el-table-column prop="totalSale" label="代售总额" width="110"> 
          </el-table-column>
          <el-table-column prop="commission" label="佣金比例(%)" width="110"> 
          </el-table-column>
          <el-table-column prop="commissionPayable" label="佣金(代理服务费)" width="110"> 
          </el-table-column>
          <el-table-column prop="note" label="备注"> 
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
        <el-dialog :title="form.title" v-model="form.visible" size="tiny">
          <el-form ref="form" :model="form.data" label-width="80px">
            <el-form-item label="用户姓名">
              <el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
            </el-form-item>
            <el-form-item label="用户性别">
              <el-select v-model="form.data.sex" placeholder="请选择性别" style="width: 100%">
                <el-option label="未知" value="unkown"></el-option>
                <el-option label="男" value="male"></el-option>
                <el-option label="女" value="female"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="用户状态">
              <el-select v-model="form.data.disable" placeholder="是否需要禁用用户" style="width: 100%">
                <el-option label="正常" value="0"></el-option>
                <el-option label="禁用" value="1"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="手机号码">
              <el-input v-model="form.data.mobilePhone" placeholder="请输入手机号码" ></el-input>
            </el-form-item>
            <el-form-item label="用户简介">
              <el-input v-model="form.data.note" placeholder="请输入用户简介" ></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="handleSubmit()">立即更新</el-button>
              <el-button @click="form.visible=false">取消</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>
      </el-col>
    </el-row>
    <div id="pdf-wrap" style="display:none;">
      <el-row :gutter="20" style="margin-bottom: 20px">
        <el-col :span='24' style="text-align: center" >
          酒店对账明细
        </el-col>
      </el-row>
      <el-row :gutter="20" style="margin-bottom: 20px;">
        <el-col :span='12' style="text-align: left; margin-top: 20px;" >
          <div style="position: relative;left: 0px;display: inline-block;width:280px;">供应商：{{strSupplierName}}</div>
          <div style="position: absolute;left: 300px;display: inline-block;">时间段：{{strStartTime}}至{{strEndTime}}</div>
          <div style="position: absolute;left: 500px;display: inline-block;">订单状态：{{strStatus}}</div>
          <div style="position: absolute;left: 700px;display: inline-block;width:230px;">支付方式：{{strPayType}}</div>
          <div style="position: absolute;left: 950px;display: inline-block;">单位：元</div>
       </el-col>
      </el-row>
      <el-row :gutter="20" >
        <el-col :span='24'>
          <table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
            <tr width="100%" style="border-bottom: none;">
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">序号</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 50px;">供应商</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">支付日期</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">支付方式</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">发货日期</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">订单号</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">交易类型</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">快递单号</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">快递名称</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 50px;">产品名称及规格</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 10px;">单位</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">发货数量</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">产品代售金额小计</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">退货总金额</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">代售总额</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">佣金比例(%)</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 30px;">佣金(代理服务费)</td>
              <td style="padding: 5px;font-size:12px;text-align:center;width: 50px;border-right: 1px solid #9a9a9a;">备注</td>
            </tr>
            <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
              <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payTime}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payType}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.sendOutTime}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.order_code}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeType}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.expressCode}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.logisticsName}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.product_name}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;"></td>
              <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.totalSale}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.refundCash}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.totalSale}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.commission}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.commissionPayable}}</td>
              <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.note}}</td>
            </tr>
            <!-- <tr width="100%" style="border-bottom: none;">
               <td style="padding: 5px;font-size:12px;text-align:center;">合计</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;"></td>
               <td style="padding: 5px;font-size:12px;text-align:center;">{{totalPayable}}</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">&nbsp;</td>
               <td style="padding: 5px;font-size:12px;text-align:center;">{{totalRefundCash}}</td>
               <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">&nbsp;</td>
            </tr>  -->
            <tr width="100%" >
              <td colspan="18" style="padding: 5px;font-size:12px;text-align:left;border-right: 1px solid #9a9a9a;">截止{{$filter(search.data.endTime,"y.m.d",0)}}，累计发货 {{totalUnitOrdered}}  单，累计退货 {{totalRefundAmount}}  单，销售总成本{{totalSaleCost}}元， 销售总额{{ totalSale}}   元；其中微信支付 {{weixin}} 元(微信App：{{weixinApp}} 元，微信网页：{{weixinPc}} 元)，支付宝支付：{{alipay}}  元，银联支付：{{unionpay}} 元</td>
            </tr> 
            <tr width="100%" >
               <td colspan="9" style="padding: 5px;font-size:12px;text-align:left;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}累计应收（付）货款：{{ totalSale}}  元</td>
               <td colspan="9"  style="padding: 5px;font-size:12px;text-align:left;border-right: 1px solid #9a9a9a;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}应开增值税普通（专用）发票累计：{{ totalSale}}   元</td>
            </tr>
            <tr width="100%">
               <td colspan="9" style="padding: 5px;font-size:12px;text-align:left; height: 50px;">甲方确认：</td>
               <td colspan="9"  style="padding: 5px;font-size:12px;text-align:left; height: 50px;border-right: 1px solid #9a9a9a;">乙方确认：</td>
            </tr>
             <tr width="100%" >
               <td colspan="9" style="padding: 5px;font-size:12px;text-align:left;border-bottom: 1px solid #9a9a9a;">日期：</td>
               <td colspan="9"  style="padding: 5px;font-size:12px;text-align:left;border-right: 1px solid #9a9a9a;border-bottom: 1px solid #9a9a9a;">日期：</td>
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
        selections: [],
      },
      loading:false,
      form: {
        visible: false,
        title: '',
        disabled: false,
        data: {
          name: '',
          sex: 'unkown',
          role: 'query',
          mobilePhone: '',
          address: ''
        }
      },
      strSupplierName: '',
      strStartTime: '',
      strEndTime: '',
      strStatus: '',
      strPayType: '',
      totalSaleCost:0,
      totalRefundAmount:0,
      totalRefundCash:0,
      totalUnitOrdered:0,
      totalSale:0,
      totalCommissionPayable: 0,
      weixin: 0,
      weixinApp: 0,
      weixinPc: 0,
      alipay: 0,
      unionpay: 0,
      search: {
        visible: false,
        useOrderCode: true,
        usePayType: true,
        useTradeNo: true,
        useStartTime: true,
        useEndTime: true,
        useSupplierId:true,
        useStatus: true,
        supplier_options:[],
        data: {
          orderCode: '',
          payType: '',
          tradeNo: '',
          startTime: '',
          endTime: '',
          supplierId: '',
          status: ''
        }
      }
    }
  },

  methods: {
    handleSelectionChange(val) {
      this.table.selections = val;
      this.handleSearch();
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
      this.form.title = "创建用户";
      this.form.disabled = false;
      this.form.data.name = '';
      this.form.data.sex = 'unkown';
      this.form.data.role = 'query';
      this.form.data.mobilePhone = '';
      this.form.data.address = '';
      this.form.visible = true;
    },
    handleEdit(id, row) {
      this.form.title = "修改用户";
      this.form.disabled = true;
      this.form.data.name = row.name;
      this.form.data.sex = row.sex;
      this.form.data.role = row.role;
      this.form.data.mobilePhone = row.mobilePhone;
      this.form.data.address = row.address;
      this.form.visible = true;
    },
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {

    },
    getSearchKey(key) {
      var me=this;
      me.searchStr = "";
      key.orderType=1;
      if(this.search.useSupplierId&&this.search.data.supplierId){
        key.supplierId=this.search.data.supplierId;
        for (var i = 0; i < me.search.supplier_options.length; i++) {
          var item = me.search.supplier_options[i];
          if (item.id == key.supplierId) {
            me.searchStr += ' 供应商: ' + item.name + '；';
            break;
          }
        }
      }
      if(this.search.useOrderCode&&this.search.data.orderCode){
        key.orderCode=this.search.data.orderCode;
        me.searchStr += "订单号：" + key.orderCode + "；";
      }
      if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
        me.searchStr += "订单状态：" + this.$getStatus(key.status) + "；";
      }
      if(this.search.usePayType&&this.search.data.payType){
        key.payType=JSON.stringify(this.search.data.payType);
        me.searchStr += '支付方式: ' + this.$getOrderPayTypes(this.search.data.payType) + "；";
      }
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime=this.$formatDate(this.search.data.startTime);
        me.searchStr += "开始时间：" + key.startTime + "；";
      }
      if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        me.searchStr += "结束时间：" + key.endTime + "；";
      }
      return key;
    },
    handleSearch() {
      var key={};
      var me = this;
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      key = this.getSearchKey(key);

      this.$get("admin/trip/list",key)
        .then2(function(response){
          for(var i=0;i<response.data.data.length;i++){
            var item = response.data.data[i];
            item.couponDiscount = 0;
            item.taxRate=item.taxRate + '%';
            item.source=me.$getOrderSource(item.source);
            item.payType=me.$getOrderPayType(item.payType);
            item.product_name=item.product_name + " " + item.selectProterties;
          }
          for(var i=0;i<response.data.list.length;i++){
            var item = response.data.list[i];
            item.couponDiscount = 0;
            item.taxRate=item.taxRate + '%';
            item.source=me.$getOrderSource(item.source);
            item.payType=me.$getOrderPayType(item.payType);
            item.product_name=item.product_name + " " + item.selectProterties;
          }
          me.table.items=response.data.data;
          me.table.list=response.data.list;
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
          me.totalCommissionPayable=response.data.totalCommissionPayable;
          me.strSupplierName = response.data.strSupplierName;
          me.strStartTime = response.data.strStartTime;
          me.strStatus = response.data.strStatus;
          me.strPayType = response.data.strPayType;
          me.search.visible = false;
      }).catch(function(err){
          console.log(err);
      });
    },
    //导出Excel表
    handleExport(){
      var me = this;
      var key = {};
      key.headerTitle = "酒店对账明细";
      key = this.getSearchKey(key);
      this.$get("admin/trip/export", key)
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
<template scoped>
  <div>
    <!--查询弹出框-->
    <el-dialog title="查询销售明细" v-model="search.visible" size="small" top="8vh">
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
            <el-col :span="5" >
              <el-checkbox label="支付方式" v-model="search.payType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" multiple v-model="search.data.payType" :disabled="!search.payType">
                <el-option label="请选择支付方式" value=""></el-option>
                <el-option label="微信支付" value="1"></el-option>
                <el-option label="支付宝支付" value="2"></el-option>
                <el-option label="银联支付" value="3"></el-option>
                <el-option label="团购卡支付" value="5"></el-option>
                <el-option label="积分兑换支付" value="6"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-input  v-model="search.data.order_code" :disabled="!search.order_code" class="bottom20 input-size">
            <el-checkbox label="订单号" slot="prepend" v-model="search.order_code"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="截止日期" v-model="search.endTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
               <el-date-picker v-model="search.data.endTime"  type="date" placeholder="截止日期" :disabled="!search.endTime">
               </el-date-picker>
            </el-col>
          </el-row>
        </el-col>
        <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5">
              <el-checkbox label="平台类型" v-model="search.source" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="search.data.source" :disabled="!search.source">
                <el-option label="请选择平台类型" value=""></el-option>
                <el-option label="PC端" value="1"></el-option>
                <el-option label="微信端" value="2"></el-option>
                <el-option label="androidApp" value="3"></el-option>
                <el-option label="苹果App" value="4"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="订单状态" v-model="search.status" style="margin-top: 5px"> 
              </el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="search.data.status" :disabled="!search.status">
                <el-option label="请选择订单状态" value=""></el-option>
                <el-option label="已支付" value="1"></el-option>
                <el-option label="已发货" value="2"></el-option>
                <el-option label="已收货" value="3"></el-option>
              </el-select>
            </el-col>
          </el-row>  
          <el-row class="bottom20">
            <el-col :span="5" >
              <el-checkbox label="开始日期" v-model="search.startTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
             <el-col :span="16">
                <el-date-picker v-model="search.data.startTime"  type="date" placeholder="开始日期" :disabled="!search.startTime">
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
      <el-col :span="4"><bc-statbox title="总退款金额" v-bind:part1="totalRefundCash" back="#58B7FF"></bc-statbox></el-col>
      <!-- <el-col :span="4"><bc-statbox title="总手续费" v-bind:part1="123" back="#20A0FF"></bc-statbox></el-col> -->
      <el-col :span="4"><bc-statbox title="总税额" v-bind:part1="totalTaxCost" back="#1D8CE0"></bc-statbox></el-col>
      <!-- <el-col :span="4"><bc-statbox title="总产品价格" v-bind:part1="totalActualProductPrice" back="#1D8CE0"></bc-statbox></el-col> -->
      <el-col :span="4"><bc-statbox title="总成本" v-bind:part1="totalCost" back="#1D8CE0"></bc-statbox></el-col>
    </el-row>

    <el-row :gutter="10">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
        <span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
        <el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="order_code" label="订单号" width="175">
          </el-table-column>
          <el-table-column prop="tradeType" label="交易类型" width="100">
          </el-table-column>
          <el-table-column prop="payTime" label="支付时间" width="200">
          </el-table-column>
          <el-table-column prop="tradeNo" label="交易号" width="200">
          </el-table-column>  
          <el-table-column prop="tradeCode" label="商户订单号" width="175">
          </el-table-column>           
           <el-table-column prop="supplierName" label="供应商" width="200">
          </el-table-column> 
          <el-table-column prop="product_name" label="产品名称" width="200">
          </el-table-column> 
          <el-table-column prop="taxRate" label="税率" width="100"> 
          </el-table-column>  
          <el-table-column prop="actualUnitPrice" label="零售单价（含税）" width="110"> 
          </el-table-column> 
          <el-table-column prop="unitOrdered" label="销售数量（产品）" width="110"> 
          </el-table-column> 
          <el-table-column prop="totalPayable" label="销售金额" width="110"> 
          </el-table-column>
          <el-table-column prop="taxRateSum" label="税额" width="110"> 
          </el-table-column>
          <el-table-column prop="couponDiscount" label="优惠金额" width="110"> 
          </el-table-column>
          <el-table-column prop="refundCash" label="退款金额" width="110"> 
          </el-table-column>
          <el-table-column prop="payType" label="收款方式" width="100">
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
                    销售明细表查询
             </el-col>
          </el-row>
          <el-row :gutter="20" style="margin-bottom: 30px;">
            <el-col :span='12' style="text-align: left; margin-top: 20px;" >
                  <div style="position: relative;left: 0px;display: inline-block;">供应商：</div>
                  <div style="position: absolute;left: 200px;display: inline-block;">时间段：{{search.data.strTime}}</div>
                  <div style="position: absolute;left: 500px;display: inline-block;">订单状态：{{search.data.strStatus}}</div>
                  <div style="position: absolute;left: 700px;display: inline-block;width:230px;">支付方式：{{search.data.strPayType}}</div>
                  <div style="position: absolute;left: 950px;display: inline-block;">单位：元</div>
            </el-col>
          </el-row>
          <el-row :gutter="20" >
            <el-col :span='24'>
              <table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">序号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 170px">交易号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 170px">商户订单号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 350px">供应商</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 430px">产品名称</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">税率</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">零售单价（含税）</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">销售数量</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">销售金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">优惠金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">退货金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;border-right: 1px solid #9a9a9a;">收款方式</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeNo}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeCode}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.supplierName}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.product_name}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRate}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.actualUnitPrice}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.couponDiscount}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.payType}}</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">合计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px"></td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px"></td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalRefundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px;border-right: 1px solid #9a9a9a;">&nbsp;</td>
                </tr> 
                <tr width="100%" style="border-bottom: none;">
                  <td colspan="13" style="padding: 5px;border-left:0px;border-bottom:0px;border-right:0px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                 <tr width="100%" style="border: none;">
                   <td colspan="6" style="padding: 5px;border: none;font-size:12px;text-align:left;">时间：{{$filter(new Date(),"",0)}}</td>
                   <td colspan="4"  style="padding: 5px;border: none;font-size:12px;text-align:left;">制表人：</td>
                   <td colspan="4"  style="padding: 5px;border: none;font-size:12px;text-align:left;">审核人：</td>
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
      totalRefundCash:0,
      totalTaxCost: 0,
      totalProductCost: 0,
      totalPayable: 0,
      totalPayRateSum: 0,
      totalActualProductPrice: 0,
      totalCost: 0,
      table: {
        total: 0,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        list: [],
        selections: [],
      },
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

      search: {
        visible: false,
        order_code: true,
        tradeType: true,
        tradeNo: true,
        tradeNum: true,
        payType: true,
        source: true,
        status: true,
        startTime: true,
        endTime: true,
        useSupplierId: true,
        
        data: {
          order_code: '',
          tradeType: '',
          tradeNo: '',
          tradeNum: '',
          payType: [],
          source: '',
          status: '',
          startTime: '',
          endTime: '',
          supplierId: '',
          strPayType: '',
          strStatus: '',
          strSource: '',
          strTime: '',
        },

        supplier_options: [],
      }
    }
  },

  methods: {
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
      var me = this;
      me.searchStr = "搜索";
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
      if(this.search.order_code&&this.search.data.order_code){
        key.order_code=this.search.data.order_code;
        me.searchStr += ' 订单号：' + key.order_code;
        if(key.order_code !==""){
          me.searchStr +=";";
        }
      }
      if(this.search.tradeType&&this.search.data.tradeType){
        key.tradeType=this.search.data.tradeType;
        me.searchStr += '2' + key.tradeType;
      }
      if(this.search.tradeNo&&this.search.data.tradeNo){
        key.tradeNo=this.search.data.tradeNo;
         me.searchStr += '3' + key.tradeNo ;
      }
      if(this.search.tradeNum&&this.search.data.tradeNum){
        key.tradeNum=this.search.data.tradeNum;
        me.searchStr += '4' + key.tradeNum;
      }
      if(this.search.payType&&this.search.data.payType.length > 0){
        key.payType = JSON.stringify(this.search.data.payType);
        me.searchStr += ' 支付方式: ' + this.$getOrderPayTypes(this.search.data.payType);
        me.search.data.strPayType = this.$getOrderPayTypes(this.search.data.payType);
        if(key.searchStr!=""){
          me.searchStr += "；"
        }
      }
      if(this.search.source&&this.search.data.source){
        key.source=this.search.data.source;
        me.searchStr += ' 平台类型：' + this.$getOrderSource(key.source) ;
        me.search.data.strSource = this.$getOrderSource(key.source);
        if(key.source!==""){
          me.searchStr +=";";
        }
      }
      if(this.search.status&&this.search.data.status){
        key.status=this.search.data.status;
        me.searchStr += ' 订单状态： ' + this.getStatus(key.status);
        me.search.data.strStatus = this.getStatus(key.status);
        if(key.status!==""){
          me.searchStr +=";";
        }
      }
      if(this.search.startTime&&this.search.data.startTime){
        key.startTime=this.$formatDate(this.search.data.startTime);
        me.searchStr += ' 开始时间 ：' + key.startTime ;
        if(key.startTime!==""){
          me.searchStr +=";";
        }
      }
      if(this.search.endTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        me.searchStr += ' 结束时间：' + key.endTime ;
        if( key.endTime!==""){
          me.searchStr +=";";
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
       
      this.$get("admin/order/orderList",key)
          .then2(function(response){
                for(var i=0;i<response.data.data.length;i++){
                  response.data.data[i].couponDiscount = 0;
                  response.data.data[i].source=me.$getOrderSource(response.data.data[i].source);
                  response.data.data[i].payType=me.$getOrderPayType(response.data.data[i].payType);
                }
                for(var i=0;i<response.data.list.length;i++){
                  response.data.list[i].couponDiscount = 0;
                  response.data.list[i].source=me.$getOrderSource(response.data.list[i].source);
                  response.data.list[i].payType=me.$getOrderPayType(response.data.list[i].payType);
                }
                me.table.items=response.data.data;
                me.table.list=response.data.list;
                me.table.total=response.data.totalRow;
                me.totalPayRateSum = response.data.totalPayRateSum;
                me.totalPayable = response.data.totalPayable;
                me.totalProductCost = response.data.totalProductCost;
                me.totalRefundCash = -response.data.totalRefundCash;
                me.totalActualProductPrice = response.data.totalActualProductPrice;
                me.totalTaxCost = response.data.totalTaxCost;
                me.totalCost = response.data.totalCost;
                me.search.visible=false;
                me.search.data.strTime = response.data.startTime + '至' + response.data.endTime;
      }).catch(function(err){
        console.log(err);
      });
    },
    handleExport(){
      var key = this.getSearchKey({});
      this.$get("admin/order/exportByOrderList", key)
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

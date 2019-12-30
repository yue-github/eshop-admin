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
              <el-select style="width: 100%;" v-model="search.data.supplierId" placeholder="请选择供应商" :disabled="!search.useSupplierId">
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
		    </el-col>
	    </el-form>
	    <div slot="footer" class="dialog-footer">
		    <el-button @click="search.visible = false">取 消</el-button>
		    <el-button type="primary" @click="handleSearch()">查询</el-button>
	    </div>
  	</el-dialog>
  	
    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="总成本" v-bind:part1="totalSaleCost" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款总数量" v-bind:part1="totalRefundAmount" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="退款总金额" v-bind:part1="totalRefundCash" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="产品总数量" v-bind:part1="totalUnitOrdered" back="#13CE66"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="总销售金额" v-bind:part1="totalSale" back="#13CE66"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons">
      	<el-button type="primary" @click="search.visible = true" style="float: right;margin-top: -20px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="sendOutTime" label="发货时间">
          </el-table-column> 
          <el-table-column prop="supplier_name" label="供应商名称">
          </el-table-column>
          <el-table-column prop="tradeType" label="交易类型">
          </el-table-column>
          <el-table-column prop="order_code" label="订单号">
          </el-table-column>  
          <el-table-column prop="product_name" label="产品名称">
          </el-table-column>           
          <el-table-column prop="totalProductCost" label="产品成本">
          </el-table-column> 
          <el-table-column prop="unitOrdered" label="产品数量">
          </el-table-column> 
          <el-table-column prop="totalPrice" label="产品金额">
          </el-table-column> 
          <el-table-column prop="totalActualDeliveryCharge" label="运费">
          </el-table-column> 
          <el-table-column prop="refundAmount" label="退款数量">
          </el-table-column> 
          <el-table-column prop="totalRefund" label="退款金额">
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
     <!--<div id="pdf-wrap" style="display:none;">
          <el-row :gutter="20" style="margin-bottom: 20px">
             <el-col :span='24' style="text-align: center" >
                  供应商销售明细表查询
             </el-col>
          </el-row>
          <el-row :gutter="20" >
            <el-col :span='24'>
              <table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">供应商</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">收货日期</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">税额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">销售金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">优惠金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">交易次数</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">退货金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">退货次数</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">支付宝支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">微信支付金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;border-right: 1px solid #9a9a9a;">银联POS支付金额</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.items" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payType}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPayable}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalCost}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalRefundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payRateSum}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalTaxCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.balance}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalCost}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.totalRefundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payRateSum}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalTaxCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;">{{item.balance}}</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">合计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalThirdPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalDiffer}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalThirdPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalDiffer}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px;border-right: 1px solid #9a9a9a;">&nbsp;</td>
                </tr> 
                <tr width="100%" style="border-bottom: none;">
                  <td colspan="12" style="padding: 5px;border-left:0px;border-bottom:0px;border-right:0px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                <tr width="100%" style="border: none;">
                   <td colspan="6" style="padding: 5px;border: none;font-size:12px;text-align:left;">时间：{{$filter(new Date(),"",0)}}</td>
                   <td colspan="6"  style="padding: 5px;border: none;font-size:12px;text-align:left;">打印人：</td>
                </tr>
              </table>
            </el-col>
          </el-row>
      </div>-->
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
                   <td colspan="10" style="padding: 5px;font-size:12px;text-align:left;">供货单位：</td>
                   <td colspan="16" style="padding: 5px;font-size:12px;text-align:left;">收货单位：广东通驿高速公路服务区有限公司</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="10" style="padding: 5px;font-size:12px;text-align:left;">联系人：</td>
                   <td colspan="16" style="padding: 5px;font-size:12px;text-align:left;">联系人：</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="10" style="padding: 5px;font-size:12px;text-align:left;">电话：</td>
                   <td colspan="16" style="padding: 5px;font-size:12px;text-align:left;">电话：020-22353742</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td colspan="10" style="padding: 5px;font-size:12px;text-align:left;">结款方式：月结30天</td>
                   <td colspan="16" style="padding: 5px;font-size:12px;text-align:left;">&nbsp;</td>
                </tr>
                <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 400px">供应商</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">支付日期</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">发货日期</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">订单号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">交易类型</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">快递单号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">快递名称</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 400px">产品名称及规格</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">单位</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px">单位成本（含税）</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">税率</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px">单位成本（未税）</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">发票类型</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">发货数量</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">快递费用</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">总成本</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px">单位产品成交价</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 200px">优惠券优惠金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px;">产品销售额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">销售总额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">退货数量</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">退货总额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">支付方式</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">备注</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.items" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplier_name}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payTime}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.sendOutTime}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.order_code}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeType}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.expressCode}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.logisticsName}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.product_name}}({{item.selectProterties}})</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.pricingUnit}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRate}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitCostNoTax}}</td>
                 	 <td style="padding: 5px;font-size:12px;text-align:center;">{{item.invoiceType}}</td>
                 	 <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
                 	 <td style="padding: 5px;font-size:12px;text-align:center;">{{item.deliveryPrice}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalCost}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.actualUnitPrice}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;"></td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalSalable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPrice}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundAmount}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundCash}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payType}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.note}}</td>
                  
                </tr>
               <!-- <tr width="100%" style="border-bottom: none;">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">合计</td>
                   <td colspan="2" style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">{{totalThirdPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">{{totalDiffer}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">{{totalThirdPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">{{totalDiffer}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;">&nbsp;</td>
                </tr> -->
                <tr width="100%" >
                  <td colspan="26" style="padding: 5px;font-size:12px;text-align:left;">截止2017-06-30，累计发货 {{totalUnitOrdered}}  单, 销售总成本{{ totalSaleCost}}  元，销售总额{{ totalSale}}   元，；其中微信支付  元(微信App： 元，微信网页： 元)，支付宝支付：  元，银联支付： 元</td>
                </tr> 
                <tr width="100%" >
                   <td colspan="9" style="padding: 5px;font-size:12px;text-align:left;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}累计应收（付）货款：  </td>
                   <td colspan="17"  style="padding: 5px;font-size:12px;text-align:left;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}应开增值税普通（专用）发票累计：   元</td>
                </tr>
                 <tr width="100%" >
                   <td colspan="9" style="padding: 5px;font-size:12px;text-align:left;">上月货款金额：</td>
                   <td colspan="17"  style="padding: 5px;font-size:12px;text-align:left;">{{$filter(search.data.startTime,"y.m.d",0)}}-{{$filter(search.data.endTime,"y.m.d",0)}}应（收）付快递成本累计：0.0</td>
                </tr>
                <tr width="100%" >
                   <td colspan="9" style="padding: 5px;font-size:12px;text-align:left;">上月开票金额：      </td>
                   <td colspan="17"  style="padding: 5px;font-size:12px;text-align:left;">上月快递成本总金额：  </td>
                </tr>

                <tr width="100%">
                   <td colspan="9" style="padding: 5px;font-size:12px;text-align:left; height: 50px;">甲方确认：</td>
                   <td colspan="17"  style="padding: 5px;font-size:12px;text-align:left; height: 50px;">乙方确认：</td>
                </tr>
                 <tr width="100%" >
                   <td colspan="9" style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
                   <td colspan="17"  style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
                </tr>
               
              </table>
            </el-col>
          </el-row>
          <el-row :gutter="20" style="margin-bottom: 20px;position: relative;">
             <el-col :span='12' style="text-align: left; margin-top: 20px;font-size: 13px;" >
         				<span style="position: relative;left: 0px;display: inline-block;">时间：{{$filter(new Date(),"",0)}}</span>
           		  <span style="position: relative;left: 500px;display: inline-block;">审核人：</span>
           		  <span style="position: absolute;left: 900px;display: inline-block;">制表人：</span>
             </el-col>
          </el-row>
           <el-row :gutter="20" style="margin-top: 30px;">
             <el-col :span='24' style="text-align: center" >
              	<table width="600px" style="float: right;" border="1" cellspacing="0" cellpadding="0">
              		  <tr width="100%" >
		                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;">说明  </td>
		                   <td colspan="10"  style="padding: 5px;font-size:12px;text-align:left;">单位成本：指单个产品的成本（不含邮费）  </td>
		                </tr>
		                <tr width="100%">
		                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;"></td>
		                   <td colspan="10"  style="padding: 5px;font-size:12px;text-align:left;"> 总成本=单位成本*发货数量+邮费</td>
		                </tr>
		                <tr width="100%" >
		                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;"></td>
		                   <td colspan="10"  style="padding: 5px;font-size:12px;text-align:left;">产品售价：指单个产品的售价</td>
		                </tr>
		               	<tr width="100%" >
		                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;"></td>
		                   <td colspan="10"  style="padding: 5px;font-size:12px;text-align:left;">产品销售额=产品售价*发货数量-优惠券优惠金额</td>
		                </tr>
		                <tr width="100%" >
		                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;"></td>
		                   <td colspan="10"  style="padding: 5px;font-size:12px;text-align:left;">销售总额=产品售价*发货数量+邮费-优惠券优惠金额</td>
		                </tr>
		                <tr width="100%" >
		                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;"></td>
		                   <td colspan="10"  style="padding: 5px;font-size:12px;text-align:left;">退货总额=退货数量*产品售价+邮费-优惠券优惠金额</td>
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
      table: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      totalSaleCost:0,
      totalRefundAmount:0,
      totalRefundCash:0,
      totalUnitOrdered:0,
      totalSale:0,
      search: {
      	visible: false,
        useStartTime: true,
        useEndTime: true,
        useSupplierName: true,
        useSupplierId:true,
        useStatus:true,
        data: {
          startTime: '',
          endTime: '',
          supplierName: '',
          supplierId: '',
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
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
    },
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {

    },
    handleSearch() {
        var me=this;
        var key={};
        key.length=this.table.pageCount ;
        key.offset= (this.table.pageIndex-1)*this.table.pageCount;
        if(this.search.useStartTime&&this.search.data.startTime){
          key.startTime=this.$formatDate(this.search.data.startTime);
        }
        if(this.search.useEndTime&&this.search.data.endTime){
          key.endTime=this.$formatDate(this.search.data.endTime);
        }
        if(this.search.useSupplierName&&this.search.data.supplierName){
          key.supplierName=this.search.data.supplierName;
        }
        if(this.search.useSupplierId&&this.search.data.supplierId){
          key.supplierId=this.search.data.supplierId;
        }
        if(this.search.useStatus&&this.search.data.status){
          key.status=this.search.data.status;
        }

        this.$get("admin/supplier/supplierOrderList",key).then2(function(response){
          me.table.items=response.data.data;
          me.table.total=response.data.totalRow;
          me.totalSaleCost=response.data.totalSaleCost;
          me.totalRefundAmount=response.data.totalRefundAmount;
          me.totalRefundCash=response.data.totalRefundCash;
          me.totalUnitOrdered=response.data.totalUnitOrdered;
          me.totalSale=response.data.totalSale;
          me.search.visible=false;
        }).catch(function(err){
          console.log(err);
        });
    },
    handleExport() {
      this.$get('admin/supplier/exportSupplierOrderList').then2(function (response) {
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
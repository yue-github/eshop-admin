<template scoped>
	<div>
		<!--查询弹出框(未审核)-->
		<el-dialog title="高级查询" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入产品名称" v-model="search.data.productName" :disabled="!search.useProductName" class="bottom20 input-size">
						<el-checkbox label="产品名称" slot="prepend" v-model="search.useProductName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="开始发货时间" v-model="search.useStartSendOutTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search.data.startSendOutTime" :disabled="!search.useStartSendOutTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="开始创建时间" v-model="search.useStartCreatedAt" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search.data.startCreatedAt" :disabled="!search.useStartCreatedAt" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>

				<el-col :span="12">
					<el-input placeholder="请输入订单号" v-model="search.data.orderCode" :disabled="!search.useOrderCode" class="bottom20 input-size">
						<el-checkbox label="订单号" slot="prepend" v-model="search.useOrderCode"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="截至发货时间" v-model="search.useEndSendOutTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search.data.endSendOutTime" :disabled="!search.useEndSendOutTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="截至创建时间" v-model="search.useEndCreatedAt" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search.data.endCreatedAt" :disabled="!search.useEndCreatedAt" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>

		<el-dialog title="高级查询" v-model="search2.visible" size="small">
			<el-form :inline="true" :model="search2" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入账单编号" v-model="search2.data.billCode" :disabled="!search2.useBillCode" class="bottom20 input-size">
						<el-checkbox label="账单编号" slot="prepend" v-model="search2.useBillCode"></el-checkbox>
					</el-input>
					<el-row v-if="tabIndex == 1" class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="开始审核时间" v-model="search2.useStartCreatedAt" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search2.data.startCreatedAt" :disabled="!search2.useStartCreatedAt" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
					<el-row v-if="tabIndex == 2" class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="开始打款时间" v-model="search2.useStartPayedAt" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search2.data.startPayedAt" :disabled="!search2.useStartPayedAt" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>

				<el-col :span="12">
					<el-input v-if="tabIndex == 1" placeholder="请输入审核人" v-model="search2.data.auditOperator" :disabled="!search2.useAuditOperator" class="bottom20 input-size">
						<el-checkbox label="审核人" slot="prepend" v-model="search2.useAuditOperator"></el-checkbox>
					</el-input>
					<el-input v-if="tabIndex == 2" placeholder="请输入打款人" v-model="search2.data.payOperator" :disabled="!search2.usePayOperator" class="bottom20 input-size">
						<el-checkbox label="打款人" slot="prepend" v-model="search2.usePayOperator"></el-checkbox>
					</el-input>
					<el-row class="bottom20" v-if="tabIndex == 1">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="截至审核时间" v-model="search2.useEndCreatedAt" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search2.data.endCreatedAt" :disabled="!search2.useEndCreatedAt" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
					<el-row class="bottom20" v-if="tabIndex == 2">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="截至打款时间" v-model="search2.useEndPayedAt" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15">
							<el-date-picker v-model="search2.data.endPayedAt" :disabled="!search2.useEndPayedAt" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search2.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch2()">查询</el-button>
			</div>
		</el-dialog>

		<el-dialog :title="formTable.title" v-model="formTable.visible" size="large">
			<el-table :data="formTable.items" border style="width: 100%">
				<el-table-column type="selection" width="55">
				</el-table-column>
				<el-table-column prop="supplier_name" label="供应商名称">
				</el-table-column>
				<el-table-column prop="product_name" label="产品名称">
				</el-table-column>
				<el-table-column prop="selectProterties" label="产品规格">
				</el-table-column>
				<el-table-column prop="order_code" label="订单编号">
				</el-table-column>
				<el-table-column prop="taxRate" label="税率">
				</el-table-column>
				<el-table-column prop="actualUnitPrice" label="成交价">
				</el-table-column>
				<el-table-column prop="unitOrdered" label="购买数量">
				</el-table-column>
				<el-table-column prop="totalActualProductPrice" label="总成交金额">
				</el-table-column>
				<el-table-column prop="unitCost" label="含税成本">
				</el-table-column>
				<el-table-column prop="unitCostNoTax" label="未含税成本">
				</el-table-column>
				<el-table-column prop="totalProductCost" label="产品总成本">
				</el-table-column>
				<el-table-column prop="sendOutTime" label="发货时间">
				</el-table-column>
				<el-table-column prop="created_at" label="创建时间">
				</el-table-column>
			</el-table>
			<el-row slot="footer">
				<el-col :span="8">
					<el-button-group>
					</el-button-group>
				</el-col>
				<el-col :span="16" style="text-align: right">
					<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="formTable.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="formTable.total">
					</el-pagination>
				</el-col>
			</el-row>
		</el-dialog>

		<el-row :gutter="20" class="bottom20">
			<el-col :span="4">
				<bc-statbox title="应付总金额" v-if="tabIndex == 0" v-bind:part1="totalActualProductPrice" back="#58B7FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="实付总金额" v-if="tabIndex == 0" v-bind:part1="trueTotalActualProductPrice" back="#58B7FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="应付总金额" v-if="tabIndex != 0" v-bind:part1="totalPayable" back="#58B7FF"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<el-button type="primary" @click="handleClickSearch" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-tabs v-model="tabIndex" @tab-click="handleClick">
						<el-tab-pane label="未审核" name="0">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column type="selection" width="55">
          						</el-table-column>
								<el-table-column prop="supplier_name" label="供应商名称">
								</el-table-column>
								<el-table-column prop="product_name" label="产品名称">
								</el-table-column>
								<el-table-column prop="selectProterties" label="产品规格">
								</el-table-column>
								<el-table-column prop="order_code" label="订单编号">
								</el-table-column>
								<el-table-column prop="taxRate" label="税率">
								</el-table-column>
								<el-table-column prop="actualUnitPrice" label="成交价">
								</el-table-column>
								<el-table-column prop="unitOrdered" label="购买数量">
								</el-table-column>
								<el-table-column prop="totalActualProductPrice" label="总成交金额">
								</el-table-column>
								<el-table-column prop="unitCost" label="含税成本">
								</el-table-column>
								<el-table-column prop="unitCostNoTax" label="未含税成本">
								</el-table-column>
								<el-table-column prop="totalProductCost" label="产品总成本">
								</el-table-column>
								<el-table-column prop="sendOutTime" label="发货时间">
								</el-table-column>
								<el-table-column prop="created_at" label="创建时间">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">审核</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>

						<el-tab-pane label="已审核" name="1">
							<el-table :data="table.items2" border style="width: 100%">
								<el-table-column prop="supplier_name" label="供应商名称">
								</el-table-column>
								<el-table-column prop="totalPayable" label="总成交金额">
								</el-table-column>
								<el-table-column prop="created_at" label="审核时间">
								</el-table-column>
								<el-table-column prop="bill_code" label="账单编号">
								</el-table-column>
								<el-table-column prop="audit_operator" label="审核人">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleDetail(scope.$index, scope.row)">查看</el-button>
										<el-button size="small" @click="handlePay(scope.$index, scope.row)">打款</el-button>
										<el-button size="small" @click="handlePrintDetail(scope.$index, scope.row)">打印</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>

						<el-tab-pane label="已打款" name="2">
							<el-table :data="table.items2" border style="width: 100%">
								<el-table-column prop="supplier_name" label="供应商名称">
								</el-table-column>
								<el-table-column prop="totalPayable" label="总成交金额">
								</el-table-column>
								<el-table-column prop="payed_at" label="打款时间">
								</el-table-column>
								<el-table-column prop="bill_code" label="账单编号">
								</el-table-column>
								<el-table-column prop="pay_operator" label="打款人">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleDetail(scope.$index, scope.row)">查看</el-button>
										<el-button size="small" @click="handlePrintDetail(scope.$index, scope.row)">打印</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
					</el-tabs>

					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<el-button type="info" size="small" @click="batchSubmit()" v-if="tabIndex == 0"><i class="icon-arrow-down-circle icons"></i> 提交</el-button>
								<el-button type="info" size="small" @click="printtf(tabIndex)"><i class="icon-printer icons"></i> 打印</el-button> 
							</el-button-group>
						</el-col>
						<el-col :span="16" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
			</el-col>
		</el-row>

		<div id="pdf-wrap" style="display:none;">
		  <el-row :gutter="20" style="margin-bottom: 20px">
		     <el-col :span='24' style="text-align: center">
		        供应商对账单（未审核）
		     </el-col>
		  </el-row>
		  <el-row :gutter="20" style="margin-bottom: 5px">
		    <el-col :span='24' style="text-align: left">
		        应付总金额：{{totalActualProductPrice}}元
		    </el-col>
		  </el-row>
		  <el-row :gutter="20" >
		    <el-col :span='24'>
		      <table width="100%" border="1" cellspacing="0" cellpadding="0">
		        <tr width="100%" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">供应商</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">产品名称</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">产品规格</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">订单编号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 20px">税率</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">成交价</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 30px">购买数量</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">总成交金额</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 30px">含税成本</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">未含税成本</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px;">产品总成本</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;">发货时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;">创建时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;" v-if="tabIndex == 1">审核时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;" v-if="tabIndex == 1">审核人</td>
		        </tr>
		        <tr width="100%" v-for="(item,index) in list" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:left;">{{item.supplier_name}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.product_name}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.selectProterties}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.order_code}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRate}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.actualUnitPrice}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalActualProductPrice}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitCost}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitCostNoTax}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalProductCost}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.sendOutTime}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.created_at}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 1">{{item.audited_at}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 1">{{item.operator}}</td>
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

		<div id="pdf-wrap2" style="display:none;">
		  <el-row :gutter="20" style="margin-bottom: 20px">
		     <el-col :span='24' style="text-align: center" v-if="tabIndex==1">
		        供应商对账单（已审核）
		     </el-col>
		     <el-col :span='24' style="text-align: center" v-if="tabIndex==2">
		        供应商对账单（已打款）
		     </el-col>
		  </el-row>
		  <el-row :gutter="20" style="margin-bottom: 5px">
		    <el-col :span='24' style="text-align: left">
		        应付总金额：{{totalPayable}}元
		    </el-col>
		  </el-row>
		  <el-row :gutter="20" >
		    <el-col :span='24'>
		      <table width="100%" border="1" cellspacing="0" cellpadding="0">
		        <tr width="100%" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 250px">供应商</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">账单编号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">总成交金额</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;" v-if="tabIndex == 1">审核时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;" v-if="tabIndex == 1">审核人</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;" v-if="tabIndex == 2">打款时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;" v-if="tabIndex == 2">打款人</td>
		        </tr>
		        <tr width="100%" v-for="(item,index) in list" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:left;">{{item.supplier_name}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.bill_code}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPayable}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 1">{{item.created_at}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 1">{{item.audit_operator}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 2">{{item.payed_at}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 2">{{item.pay_operator}}</td>
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
		</div>

		<div id="pdf-wrap3" style="display:none;">
		  <el-row :gutter="20" style="margin-bottom: 20px">
		     <el-col :span='24' style="text-align: center" v-if="tabIndex==1">
		        供应商对账单（已审核）
		     </el-col>
		     <el-col :span='24' style="text-align: center" v-if="tabIndex==2">
		        供应商对账单（已打款）
		     </el-col>
		  </el-row>
		  <el-row :gutter="20" >
		    <el-col :span='24'>
		      <table width="100%" border="1" cellspacing="0" cellpadding="0">
		      	<tr>
		      		<td colspan="8" style="padding: 5px;font-size:12px;text-align:center;width: 60px">账单汇总</td>
		      	</tr>
		        <tr width="100%" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 250px">供应商</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">账单编号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">总成交金额</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;" v-if="tabIndex == 1">审核时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;" v-if="tabIndex == 1">审核人</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;" v-if="tabIndex == 2">打款时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 100px;" v-if="tabIndex == 2">打款人</td>
		        </tr>
		        <tr width="100%" v-for="(item,index) in supplierBill" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:left;">{{item.supplier_name}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.bill_code}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPayable}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 1">{{item.created_at}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 1">{{item.audit_operator}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 2">{{item.payed_at}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;" v-if="tabIndex == 2">{{item.pay_operator}}</td>
		        </tr>
		      </table>
		      <br/>
		      <table width="100%" border="1" cellspacing="0" cellpadding="0">
		      	<tr>
		      		<td colspan="16" style="padding: 5px;font-size:12px;text-align:center;width: 60px">账单明细</td>
		      	</tr>
		        <tr width="100%" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px">序号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">供应商</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">产品名称</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 50px">产品规格</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">订单编号</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 20px">税率</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">成交价</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 30px">购买数量</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">总成交金额</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 30px">含税成本</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px">未含税成本</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 40px;">产品总成本</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;">发货时间</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;width: 60px;">创建时间</td>
		        </tr>
		        <tr width="100%" v-for="(item,index) in supplierBillDetails" style="border-bottom: none;">
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:left;">{{item.supplier_name}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.product_name}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.selectProterties}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.order_code}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRate}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.actualUnitPrice}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitOrdered}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalActualProductPrice}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitCost}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.unitCostNoTax}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalProductCost}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.sendOutTime}}</td>
		           <td style="padding: 5px;font-size:12px;text-align:center;">{{item.created_at}}</td>
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
		</div>
	</div>
</template>

<script>
	module.exports = {
		data: function() {
			return {
				tabIndex: 0,
				totalActualProductPrice: 0,
				trueTotalActualProductPrice: 0,
				totalPayable: 0,
				list: [],
				supplierBill:[],
				supplierBillDetails:[],
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '',
						supplier_name: '',
						product_name: '',
						selectProterties: '',
						actualUnitPrice: '',
						unitOrdered: '',
						totalActualProductPrice: '',
						unitCost: '',
						unitCostNoTax: '',
						totalProductCost: '',
						taxRate: '',
						sendOutTime: '',
						created_at: '',
						code:'',
						audited_at:'',
						operator:'',
						order_code:'',
					},],
					items2: [{
						id: '',
						supplier_id: '',
						supplier_name: '',
						totalPayable: '',
						bill_code: '',
						created_at: '',
						payed_at: '',
						audit_operator: '',
						status: '',
						pay_operator: '',
					},],
					selections: [],
				},
				formTable: {
					visible: false,
					title: '账单详情',
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '',
						supplier_name: '',
						product_name: '',
						selectProterties: '',
						actualUnitPrice: '',
						unitOrdered: '',
						totalActualProductPrice: '',
						unitCost: '',
						unitCostNoTax: '',
						totalProductCost: '',
						taxRate: '',
						sendOutTime: '',
						created_at: '',
						code:'',
						audited_at:'',
						operator:'',
						order_code:'',
					},],
				},
				search: {
					visible: false,
					useProductName: true,
					useOrderCode: true,
					useStartSendOutTime: true,
					useEndSendOutTime: true,
					useStartCreatedAt: true,
					useEndCreatedAt: true,
					useStartAuditedAt: true,
					useEndAuditedAt: true,
					useOperator: true,
					data: {
						productName: '',
						orderCode: '',
						startSendOutTime: '',
						endSendOutTime: '',
						startCreatedAt: '',
						endCreatedAt: '',
						startAuditedAt: '',
						endAuditedAt: '',
						operator: '',
					},
					supplier_options:[],
				},
				search2: {
					visible: false,
					useBillCode: true,
					useAuditOperator: true,
					usePayOperator: true,
					useStartCreatedAt: true,
					useEndCreatedAt: true,
					useStartPayedAt: true,
					useEndPayedAt: true,
					data: {
						billCode: '',
						auditOperator: '',
						payOperator: '',
						startCreatedAt: '',
						endCreatedAt: '',
						startPayedAt: '',
						endPayedAt: '',
					},
				}
			}
		},
		methods: {
			handleSelectionChange(val) {
				this.table.selections = val;
				// 计算实付总金额
				this.trueTotalActualProductPrice = 0;
				for (var i = 0; i < val.length; i++) {
					this.trueTotalActualProductPrice += val[i].totalActualProductPrice;
				}
			},
			handleSizeChange(val) {
				this.table.pageCount = val;
				this.handleSearch();
			},
			handleCurrentChange(val) {
				this.table.pageIndex = val;
				this.handleSearch();
			},
			handleClickSearch() {
				if (this.tabIndex == 0) {
					this.search.visible = true;
				} else {
					this.search2.visible = true;
				}
			},
			batchSubmit() {
				var me = this;
				var selections = me.table.selections;
				var ids = [];
				for (var i = 0; i < selections.length; i++) {
					ids.push(selections[i].id);
				}

				this.$confirm('是否提交选中的供应商账单?', '提示', {
		          confirmButtonText: '确定',
		          cancelButtonText: '取消',
		          type: 'warning'
		        }).then(() => {
		          me.$post("admin/supplierBill/batchSubmit", {ids:JSON.stringify(ids)}).then2(function(response) {
					if (response.data.error == 0) {
						me.$notify({
				          title: '成功',
				          message: '操作成功',
				          type: 'success'
				        });
				        me.handleSearch();
					} else {
						me.$notify.error({
				          title: '错误',
				          message: '操作失败'
				        });
					}
				  }).catch(function(err) { 
					console.log(err);
				  });
		        }).catch(() => {
		                 
		        });
			},
			handleEdit(id, row) {
				var me = this;
				this.$confirm('是否提交该供应商账单?', '提示', {
		          confirmButtonText: '确定',
		          cancelButtonText: '取消',
		          type: 'warning'
		        }).then(() => {
		          me.$post("admin/supplierBill/submit", {id:row.id}).then2(function(response) {
					if (response.data.error == 0) {
						me.$notify({
				          title: '成功',
				          message: '操作成功',
				          type: 'success'
				        });
				        me.handleSearch();
					} else {
						me.$notify.error({
				          title: '错误',
				          message: '操作失败'
				        });
					}
				  }).catch(function(err) { 
					console.log(err);
				  });
		        }).catch(() => {
		                 
		        });
			},
			handleDetail(id, row) {
				var me = this;
				me.formTable.visible = true;
				var key = {};
				key.length = me.formTable.pageCount;
				key.offset = (me.formTable.pageIndex - 1) * me.formTable.pageCount;
				key.supplierBillId = row.id;

				me.$get('admin/supplierBill/many', key).then2(function(response) {
					me.formTable.items = response.data.data;
					me.formTable.total = response.data.totalRow;
				}).catch(function(err) {
					console.log(err);
				});
			},
			handlePay(id, row) {
				var me = this;
				me.$confirm('是否确认打款?', '提示', {
		            confirmButtonText: '确定',
		            cancelButtonText: '取消',
		            type: 'warning'
		        }).then(() => {
		            me.$post('admin/supplierBill/paySupplierBill', {id:row.id}).then2(function(response) {
						if (response.data.error == 0) {
							me.$notify({
					          title: '成功',
					          message: '提交成功',
					          type: 'success'
					        });
					        me.handleSearch2();
						} else {
							me.$notify.error({
					          title: '错误',
					          message: '提交失败'
					        });
						}
					}).catch(function(err) {
						console.log(err);
					});
		        }).catch(() => {
		            console.log('取消');     
		        });
			},
			handleClick(tab, event) {
				this.tabIndex = tab.$options.propsData.name;
				this.table.pageIndex = 1;
				this.table.pageCount = 25;
				this.search.data = {};
				if (this.tabIndex == 0) {
					this.search.data = {};
					this.handleSearch();
				} else if (this.tabIndex == 1) {
					this.search2.data = {};
					this.handleSearch2();
				} else if (this.tabIndex == 2) {
					this.search2.data = {};
					this.handleSearch2();
				}
			},
			handleSearch() {
				var me = this;
				var key = {};
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				key.supplierId = this.$route.query.supplierId;

				if(me.search.useProductName && me.search.data.productName) {
					key.productName = me.search.data.productName;
				}
				if(me.search.useOrderCode && me.search.data.orderCode) {
					key.orderCode = me.search.data.orderCode;
				}
				if(me.search.useStartSendOutTime && me.search.data.startSendOutTime) {
					key.startSendOutTime = me.$formatDate(me.search.data.startSendOutTime);
				}
				if(me.search.useEndSendOutTime && me.search.data.endSendOutTime) {
					key.endSendOutTime = me.$formatDate(me.search.data.endSendOutTime);
				}
				if(me.search.useStartCreatedAt && me.search.data.startCreatedAt) {
					key.startCreatedAt = me.$formatDate(me.search.data.startCreatedAt);
				}
				if(me.search.useEndCreatedAt && me.search.data.endCreatedAt) {
					key.endCreatedAt = me.$formatDate(me.search.data.endCreatedAt);
				}
				if(me.search.useStartAuditedAt && me.search.data.startAuditedAt) {
					key.startAuditedAt = me.$formatDate(me.search.data.startAuditedAt);
				}
				if(me.search.useEndAuditedAt && me.search.data.endAuditedAt) {
					key.endAuditedAt = me.$formatDate(me.search.data.endAuditedAt);
				}
				if(me.search.useOperator && me.search.data.operator) {
					key.operator = me.search.data.operator;
				}

				me.$get('admin/supplierBill/many?status=apply', key).then2(function(response) {
					me.table.items = response.data.data;
					me.table.total = response.data.totalRow;
					me.totalActualProductPrice = response.data.totalActualProductPrice;
					me.list = response.data.list;
					me.search.visible = false;
				}).catch(function(err) {
					console.log(err);
				});
			},
			handleSearch2() {
				var me = this;
				var key = {};
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				key.supplierId = this.$route.query.supplierId;

				if (this.tabIndex == 1) {
					key.status = 'unpay';
				} else if (this.tabIndex == 2) {
					key.status = 'payed';
				}

				if(me.search2.useBillCode && me.search2.data.billCode) {
					key.billCode = me.search2.data.billCode;
				}
				if(me.search2.useAuditOperator && me.search2.data.auditOperator) {
					key.auditOperator = me.search2.data.auditOperator;
				}
				if(me.search2.usePayOperator && me.search2.data.payOperator) {
					key.payOperator = me.search2.data.payOperator;
				}
				if(me.search2.useStartCreatedAt && me.search2.data.startCreatedAt) {
					key.startCreatedAt = me.$formatDate(me.search2.data.startCreatedAt);
				}
				if(me.search2.useEndCreatedAt && me.search2.data.endCreatedAt) {
					key.endCreatedAt = me.$formatDate(me.search2.data.endCreatedAt);
				}
				if(me.search2.useStartPayedAt && me.search2.data.startPayedAt) {
					key.startPayedAt = me.$formatDate(me.search2.data.startPayedAt);
				}
				if(me.search2.useEndPayedAt && me.search2.data.endPayedAt) {
					key.endPayedAt = me.$formatDate(me.search2.data.endPayedAt);
				}

				me.$get('admin/supplierBill/supplierBills', key).then2(function(response) {
					me.table.items2 = response.data.data;
					me.table.total = response.data.totalRow;
					me.totalPayable = response.data.totalPayable;
					me.list = response.data.list;
					me.search2.visible = false;
				}).catch(function(err) { 
					console.log(err);
				});
			},
			handleExport() {
				
			},
			getAllSupplier(){
		        var me = this;
		        me.$get("admin/supplier/allSuppliers",{}).then2(function(response){
		          me.search.supplier_options = response.data.data;
		        }).catch(function(err){
		          console.log(err);
		        });
		    },
		    handlePrintDetail(index, row) {
		    	var me = this;

				me.$get('admin/supplierBill/supplierBillAndDetails', {supplierBillId: row.id}).then2(function(response) {
					if (response.data.error == 0) {
						me.supplierBill = [];
						me.supplierBill.push(response.data.supplierBill);
						me.supplierBillDetails = response.data.supplierBillDetails;
						me.$nextTick(function () {
				            me.bundprint("pdf-wrap3");
				        })
					}
				}).catch(function(err) {
					console.log(err);
				});
		    },
		    printtf:function(tabIndex){
		    	if (tabIndex == 0) {
		    		this.bundprint('pdf-wrap');
		    	} else {
		    		this.bundprint('pdf-wrap2');
		    	}
		    },
		},
		mounted: function() {
			this.handleSearch();
			this.getAllSupplier();
		}
	}
</script>
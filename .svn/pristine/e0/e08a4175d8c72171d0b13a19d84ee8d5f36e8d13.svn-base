<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="高级查询" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入商户名称" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="商户名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入产品名称" v-model="search.data.productName" :disabled="!search.useProductName" class="bottom20 input-size">
						<el-checkbox label="产品名称" slot="prepend" v-model="search.useProductName"></el-checkbox>
					</el-input>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="开始申请时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>

				</el-col>

				<el-col :span="12">

					<el-input placeholder="请输入订单号" v-model="search.data.orderCode" :disabled="!search.useOrderCode" class="bottom20 input-size">
						<el-checkbox label="订单号" slot="prepend" v-model="search.useOrderCode"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入审核人" v-model="search.data.operator" :disabled="!search.useOperator" class="bottom20 input-size">
						<el-checkbox label="审核人" slot="prepend" v-model="search.useOperator"></el-checkbox>
					</el-input>
					
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="截止申请时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>

				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<el-row :gutter="20" class="bottom20">
			<el-col :span="4">
				<bc-statbox title="交易总额" v-bind:part1="totalRefundCash" back="#58B7FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="退款总额" v-bind:part1="totalTradeCash" back="#20A0FF"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-tabs v-model="tabIndex" @tab-click="handleClick">

						<el-tab-pane label="未审核" name="0">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="orderCode" label="订单号">
								</el-table-column>
								<el-table-column prop="name" label="商户名称">
								</el-table-column>
								<el-table-column prop="created_at" label="申请时间">
								</el-table-column>
								<el-table-column prop="productName" label="产品名称">
								</el-table-column>
								<el-table-column prop="tradeCash" label="交易金额">
								</el-table-column>
								<el-table-column prop="refundCash" label="退款金额">
								</el-table-column>
								<el-table-column prop="status" label="状态">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">审核</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>

						<el-tab-pane label="通过" name="1">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="orderCode" label="订单号">
								</el-table-column>
								<el-table-column prop="name" label="商户名称">
								</el-table-column>
								<el-table-column prop="created_at" label="申请时间">
								</el-table-column>
								<el-table-column prop="refundTime" label="处理时间">
								</el-table-column>
								<el-table-column prop="productName" label="产品名称">
								</el-table-column>
								<el-table-column prop="tradeCash" label="交易金额">
								</el-table-column>
								<el-table-column prop="refundCash" label="退款金额">
								</el-table-column>
								<el-table-column prop="status" label="状态">
								</el-table-column>
								<el-table-column prop="operator" label="审核人">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row,1)">审核</el-button>
										<!--           <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button> -->
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>

						<el-tab-pane label="未通过" name="2">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="orderCode" label="订单号">
								</el-table-column>
								<el-table-column prop="name" label="商户名称">
								</el-table-column>
								<el-table-column prop="created_at" label="申请时间">
								</el-table-column>
								<el-table-column prop="refundTime" label="处理时间">
								</el-table-column>
								<el-table-column prop="productName" label="产品名称">
								</el-table-column>
								<el-table-column prop="tradeCash" label="交易金额">
								</el-table-column>
								<el-table-column prop="refundCash" label="退款金额">
								</el-table-column>
								<el-table-column prop="status" label="状态">
								</el-table-column>
								<el-table-column prop="operator" label="审核人">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">审核</el-button>
										<!--           <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button> -->
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>

						<el-tab-pane label="已打款" name="3">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="orderCode" label="订单号">
								</el-table-column>
								<el-table-column prop="name" label="商户名称">
								</el-table-column>
								<el-table-column prop="created_at" label="申请时间">
								</el-table-column>
								<el-table-column prop="refundTime" label="处理时间">
								</el-table-column>
								<el-table-column prop="remitTime" label="退款时间">
								</el-table-column>
								<el-table-column prop="productName" label="产品名称">
								</el-table-column>
								<el-table-column prop="tradeCash" label="交易金额">
								</el-table-column>
								<el-table-column prop="refundCash" label="退款金额">
								</el-table-column>
								<el-table-column prop="status" label="状态">
								</el-table-column>
								<el-table-column prop="operator" label="审核人">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row, 3)">查看</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
					</el-tabs>

					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 禁用
								</el-button>-->
								<el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<!-- <el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button> -->
								<!--<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>-->
							</el-button-group>
						</el-col>
						<el-col :span="16" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>

				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="small">
					<el-form ref="form" :model="form.data" label-width="120px">
						<div>
							<hr>
						</div>
						<el-row>
							<el-col :span="12">
								<el-form-item label="订单号" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.order_code" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="支付方式" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.payType" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="姓名" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.receiveName" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="所在省" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.receiveProvince" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="所在区" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.receiveDistrict" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="订单总邮费" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.deliveryPrice" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="商户订单号" :label-width="formLabelWidth">
									<el-input v-model="form.data.order.tradeCode" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="订单状态" :label-width="formLabelWidth">
									<el-input v-model="form.data.status" :disabled="form.disabled"></el-input>
								</el-form-item>
							</el-col>
							<el-col :span="12">
								<el-form-item label="客户手机号">
									<el-input v-model="form.data.order.preferredContactPhone" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="下单时间">
									<el-input v-model="form.data.order.orderTime" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="手机号">
									<el-input v-model="form.data.order.preferredContactPhone" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="所在市">
									<el-input v-model="form.data.order.receiveCity" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="详细地址">
									<el-input v-model="form.data.order.receiveDetailAddress" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="订单总额">
									<el-input v-model="form.data.order.totalPayable" :disabled="form.disabled"></el-input>
								</el-form-item>
								<el-form-item label="交易流水号">
									<el-input v-model="form.data.order.tradeNo" :disabled="form.disabled"></el-input>
								</el-form-item>
							</el-col>
						</el-row>
						<div>
							<hr>
						</div>

						<template>
							<el-table :data="form.data.productOrder" style="width: 100%">
								<el-table-column label="产品图片">
									<template slot-scope="props">
										<el-form label-position="left" inline class="demo-table-expand">
											<el-form-item>
												<img v-if="props.row.mainPic" style="width: 100%;" v-bind:src="props.row.mainPic" class="image">
											</el-form-item>
										</el-form>
									</template>
								</el-table-column>
								<el-table-column label="产品名称" prop="category_name">
								</el-table-column>
								<el-table-column label="单价" prop="totalActualProductPrice">
								</el-table-column>
								<el-table-column label="邮费" prop="totalDeliveryCost">
								</el-table-column>
								<el-table-column label="成本" prop="unitCost">
								</el-table-column>
								<el-table-column label="数量" prop="unitOrdered">
								</el-table-column>
								<el-table-column label="小计" prop="totalPrice">
								</el-table-column>
								<el-table-column label="明细状态" prop="status">
								</el-table-column>
							</el-table>
						</template>
						<br>
						<el-form-item label="总退款金额">
							<el-input v-model="form.data.refundCash" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="申请退款金额">
							<el-input v-model="form.data.applyCash" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="退款运费">
							<el-input v-model="form.data.deliveryPrice" :disabled="tabIndex==3"></el-input>
							<!-- <el-input v-model="(form.data.order.totalPayable-form.data.applyCash).toFixed(1)" :disabled="form.disabled"></el-input> -->
							<span v-if='tabIndex != 3'>退款运费(最高可退{{(form.data.order.totalPayable-form.data.applyCash).toFixed(1)}}元)</span>
							<!-- <span v-if='tabIndex != 3'>退款运费(最多可退{{form.data.allowedDeliveryPrice}}元)</span> -->
						</el-form-item>
						<el-form-item label="退款原因">
							<el-input v-model="form.data.reason" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="退款说明">
							<el-input v-model="form.data.note" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="备注">
							<el-input v-model="form.data.refuseNote"></el-input>
						</el-form-item>
						<el-form-item label="审核是否通过">
							<el-select style="width: 100%" v-model="form.data.status" @change="formChangeFun">
								<el-option label="请选择" value="0"></el-option>
								<el-option label="通过" value="1" :disabled="form.useStatus"></el-option>
								<el-option label="不通过" value="2"></el-option>
							</el-select>
						</el-form-item>
						<br>
						<el-form-item style="text-align: right">
							<el-button v-if="type == 1 && accept == 1" size="small" @click="handleSubmit()">打款</el-button>
							<el-button v-if="type == 0||type == 1" size="small" @click="handlCheckSubmit()">审核</el-button>
							<el-button @click="form.visible=false">关闭</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
			</el-col>

		</el-row>
	</div>
</template>

<script>
	module.exports = {
		data: function() {
			return {
				searchStr: '',
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '1',
						orderCode: '101',
						name: 'hhh',
						created_at: '2018-02-05',
						refundTime: '2018-02-06',
						remitTime: '2018-02-06',
						productName: 'god',
						tradeCash: '5000',
						refundCash: '5000',
						status: '待审核',
						operator: 'hahasd'
					}, ],
					selections: [],
				},
				type: 0,
				accept: 0,
				tabIndex: 0,
				totalRefundCash: 0,
				totalTradeCash: 0,
				form: {
					visible: false,
					title: '',
					disabled: true,
					useStatus: false,
					data: {
						accountName: '',
						accountType: 'unkown',
						accoutNumber: 'query',
						allowedApplyCash: '',
						allowedDeliveryPrice: '',
						applyCash: '',
						approvedBY: '',
						bankBranch: '',
						bankCity_id: '',
						bankName: '',
						bankProvince_id: '',
						billingAddress_id: '',
						contactNumber: '',
						couponDiscount: '',
						created_at: '',
						customerName: '',
						customerPhone: '',
						customer_id: '',
						deliveryPrice: '',
						employee_id: '',
						expirationDate: '',
						id: '',
						isGeted: '',
						note: '',
						operator: '',
						product_order_id: '',
						reason: '',
						referenceNumber: '',
						refundAmount: '',
						refundCash: '',
						refundTime: '',
						refundType: '',
						refund_code: '',
						refuseNote: '',
						remitTime: '',
						service_order_id: '',
						shop_id: '',
						status: '',
						successTime: '',
						tradeCash: '',
						updated_at: '',
						order: [{
							address_id: '',
							cancelTime: '',
							cancel_note: '',
							cancel_reason: '',
							cardType: '',
							codeType: '',
							completeTime: '',
							couponDiscount: '',
							created_at: '',
							customerCouponId: '',
							customer_id: '',
							deliveryPrice: '',
							expressCode: '',
							expressDetail: '',
							expressSubscribeStatus: '',
							expressType: '',
							gold: '',
							id: '',
							invoiceHead: '',
							isFullyPaid: '',
							isOrganizationOrder: '',
							logisticsName: '',
							note: '',
							orderTime: '',
							order_code: '',
							payRate: '',
							payRateSum: '',
							payTime: '',
							payType: '',
							payment_id: '',
							preferredContactPhone: '',
							preferred_employee_id: '',
							priority: '',
							promDiscount: '',
							promId: '',
							promitionType: '',
							promotion_id: '',
							receiveCity: '',
							receiveDetailAddress: '',
							receiveDistrict: '',
							receiveName: '',
							receiveProvince: '',
							receiveTime: '',
							salesman_id: '',
							sendOutTime: '',
							shop_id: '',
							source: '',
							status: '',
							theSameOrderNum: '',
							totalPayable: '',
							tradeCode: '',
							tradeNo: '',
							updated_at: ''
						}],
						productOrder: [{
							actualUnitDeliveryCharge: '',
							actualUnitPrice: '',
							category_name: '',
							couponDiscount: '',
							created_at: '',
							id: '',
							mainPic: '',
							minAllowableUnitDeliveryCharge: '',
							minAllowableUnitPrice: '',
							model: '',
							order_id: '',
							priceId: '',
							pricingUnit: '',
							product_id: '',
							product_name: '',
							product_summary: '',
							salesCommission: '',
							selectProterties: '',
							status: '',
							suggestedRetailUnitDeliveryCharge: '',
							suggestedRetailUnitPrice: '',
							supplier_id: '',
							supplier_name: '',
							taxId: '',
							taxName: '',
							taxRate: '',
							totalActualDeliveryCharge: '',
							totalActualProductPrice: '',
							totalCost: '',
							totalDeliveryCost: '',
							totalPrice: '',
							totalProductCost: '',
							unitCost: '',
							unitDeliverCost: '',
							unitOrdered: '',
							upc: '',
							updated_at: ''
						}]
					}
				},
				search: {
					visible: false,
					useOrderCode: true,
					useName: true,
					useTimeType: true,
					useStartTime: true,
					useEndTime: true,
					useProductName: true,
					useTradeCash: true,
					useRefundCash: true,
					useStatus: true,
					useOperator: true,
					data: {
						orderCode: '',
						name: '',
						timeType: '',
						startTime: '',
						endTime: '',
						productName: '',
						tradeCash: '',
						refundCash: '',
						status: '',
						operator: ''
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
			handleEdit(id, row, type) {
				var me = this;
				if(type === 3) {
					me.type = type;
				} else if(type === 1) {
					me.type = type;
				} else {
					me.type = 0;
				}
				me.form.title = "订单审核";
				me.form.visible = true;
				me.form.useStatus = false;

				this.$get('admin/refund/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(me.form.data.order.payType == 1) {
							me.form.data.order.payType = "微信支付";
						} else if(me.form.data.order.payType == 2) {
							me.form.data.order.payType = "支付宝";
						} else if(me.form.data.order.payType == 3) {
							me.form.data.order.payType = "银联支付";
						} else if(me.form.data.order.payType == 4) {
							me.form.data.order.payType = "钱包支付";
						}
						switch(me.form.data.order.status) {
							case 1:
								me.form.data.order.status = "待付款";
								break;
							case 2:
								me.form.data.order.status = "待发货";
								break;
							case 3:
								me.form.data.order.status = "待收货";
								break;
							case 4:
								me.form.data.order.status = "待评价";
								break;
							case 5:
								me.form.data.order.status = "已评价";
								break;
							case 6:
								me.form.data.order.status = "已取消";
								break;
							case 7:
								me.form.data.order.status = "订单完成";
								break;
						}

						if(type === 1) {
							me.form.useStatus = true;
							me.form.data.status = "通过"

						} else {

							switch(me.form.data.status) {
								case 1:
									me.form.data.status = "通过";
									break;
								case 2:
									me.form.data.status = "不通过";
									break;
								case 3:
									me.form.data.status = "已退款";
									break;
								default:
									me.form.data.status = "请选择";
									break;
							}
						}

						for(var i = 0; i < me.form.data.productOrder.length; i++) {
							switch(me.form.data.productOrder[i].status) {
								case 1:
									if(type != 1) {
										me.form.data.productOrder[i].status = "正常";
									}
									break;
								case 2:
									me.form.data.productOrder[i].status = "申请退款";
									break;
								case 3:
									me.form.data.productOrder[i].status = "不通过";
									break;
								case 4:
									me.form.data.productOrder[i].status = "退款中";
									break;
								case 5:
									me.form.data.productOrder[i].status = "退款成功";
									break;
								case 6:
									me.form.data.productOrder[i].status = "已退货";
									break;
								case 7:
									me.form.data.productOrder[i].status = "已评价";
									break;
								case 8:
									me.form.data.productOrder[i].status = "退款失败";
									break;
							}
						}
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			handleDelete(id, row) {

			},
			handleDeleteSelections() {

			},
			handleClick(tab, event) {
				this.tabIndex = tab.$options.propsData.name;
				this.table.pageIndex = 1;
				this.table.pageCount = 25;
				this.handleSearch();
			},
			handlCheckSubmit() {
				var me = this;
				var key = {};
				var data = me.form.data;
				if (data.deliveryPrice === '' || typeof(data.deliveryPrice) == "undefined") {
					me.$notify({
						title: '警告',
						message: '退款运费不能为空',
						type: 'warning'
					});
					return false;
				}
				if (data.deliveryPrice < 0) {
					me.$notify({
						title: '警告',
						message: '退款运费不能小于0',
						type: 'warning'
					});
					return false;
				}
				if (data.deliveryPrice > data.allowedDeliveryPrice) {
					me.$notify({
						title: '警告',
						message: '退款运费(最多可退'+data.allowedDeliveryPrice+'元)',
						type: 'warning'
					});
					return false;
				}
				if(data.status === '请选择') {
					me.$notify({
						title: '警告',
						message: '请选择审核状态！',
						type: 'warning'
					});
					return false;
				}
				key.id = data.id;
				key.status = data.status;
				data.deliveryPrice = me.form.data.order.totalPayable-me.form.data.applyCash;
				// 'admin/refund/check'
				this.$post('admin/refund/check', data).then2(function(response) {
					me.$notify({
						title: '成功',
						message: '保存成功',
						type: 'success'
					});
					me.form.visible = false;
					me.handleSearch();
				}, function(response) {
					me.$notify.error({
						title: '错误',
						message: '保存失败',
					});
					me.form.visible = false;
				});
			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				console.log(1212111111111)
				// 上传的退款运费
				console.log(data.deliveryPrice);
				// data.deliveryPrice = me.form.data.order.totalPayable-me.form.data.applyCash;
				data.status = 1;
				me.$confirm('您现在选择的是打款功能, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					// admin/refund/check
					this.$post('admin/refund/audit', data).then2(function(response) {
						if(response.data.error == 0) {
							me.$notify({
								title: '成功',
								message: '保存成功',
								type: 'success'
							});
						} else {
							me.$notify({
								title: '失败',
								message: '保存失败'
							});
						}

						me.form.visible = false;
						me.handleSearch();
					}, function(response) {
						me.$notify.error({
							title: '错误',
							message: '保存失败',
						});
						me.form.visible = false;
					});
				}).catch(() => {

				});
			},
			getSearchKey(key) {
				var me = this;
				
				me.searchStr = '搜索';
				if(this.search.useOrderCode && this.search.data.orderCode) {
					key.orderCode = this.search.data.orderCode;
					me.searchStr += ' 订单号： ' + key.orderCode;
					if(key.orderCode != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += ' 商户名称： ' + key.name ;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useStartTime && this.search.data.startTime && this.search.useTimeType) {
					key.startTime = this.$formatDate(this.search.data.startTime);
					me.searchStr += ' 开始申请时间： ' + key.startTime;
					if(key.startTime != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime && this.search.useTimeType) {
					key.endTime = this.$formatDate(this.search.data.endTime);
					me.searchStr += ' 结束申请时间： ' + key.endTime ;
					if(key.endTime != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useProductName && this.search.data.productName) {
					key.productName = this.search.data.productName;
					me.searchStr += ' 产品名称： ' + key.productName;
					if(key.productName != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useTradeCash && this.search.data.tradeCash) {
					key.tradeCash = this.search.data.tradeCash;
					me.searchStr += ' 交易金额： ' + key.tradeCash;
					if(key.tradeCash != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useRefundCash && this.search.data.refundCash) {
					key.refundCash = this.search.data.refundCash;
					me.searchStr += ' 退款金额： ' + key.refundCash;
					if(key.refundCash != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useOperator && this.search.data.operator) {
					key.operator = this.search.data.operator;
					me.searchStr += '审核人：' + key.operator;
					if(key.operator != '') {
						me.searchStr += '；';
					}
				}
				return key;
			},
			handleSearch() {
				if(this.tabIndex == 0) {
					url = 'admin/refund/many?status=0';
				} else if(this.tabIndex == 1) {
					url = 'admin/refund/many?status=1';
				} else if(this.tabIndex == 2) {
					url = 'admin/refund/many?status=2';
				} else {
					url = 'admin/refund/many?status=3';
				}
				var me = this;
				var key = {};
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				key = me.getSearchKey(key);
				this.$get(url, key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.totalTradeCash = response.data.totalTradeCash;
						me.totalRefundCash = response.data.totalRefundCash;
						for(var i = 0; i < me.table.items.length; i++) {
							switch(me.table.items[i].status) {
								case 0:
									me.table.items[i].status = "待审核";
									break;
								case 1:
									me.table.items[i].status = "退款中";
									break;
								case 2:
									me.table.items[i].status = "审核不通过";
									break;
								case 3:
									me.table.items[i].status = "退款成功";
									break;
								case 4:
									me.table.items[i].status = "退款失败";
									break;
							}
						}
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
			},

			handleExport() {
				if(this.tabIndex == 0) {
					url = 'admin/refund/exportRefund?status=0';
				} else if(this.tabIndex == 1) {
					url = 'admin/refund/exportRefund?status=1';
				} else if (this.tabIndex == 2) {
					url = 'admin/refund/exportRefund?status=2';
				} else {
					url = 'admin/refund/exportRefund?status=3';
				}
				var key = {};
				key = this.getSearchKey(key);
				
				this.$get(url, key)
					.then2(function(response) {
						if(response.data.error == 0) {
							window.open(response.data.path);
						} else {
							return false;
						}
					}).catch(function(err) { 
						console.log(err);
					});
			},
			formChangeFun(a) {
				if(a == 2) {
					this.accept = 0;
				} else {
					this.accept = 1;
				}
			}

		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
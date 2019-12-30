<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="高级查询" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入订单号" v-model="search.data.code" :disabled="!search.useCode" class="bottom20 input-size">
						<el-checkbox label="订单号" slot="prepend" v-model="search.useCode"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入商户名称" v-model="search.data.shopName" :disabled="!search.useShopName" class="bottom20 input-size">
						<el-checkbox label="商户名称" slot="prepend" v-model="search.useShopName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入客户手机号" v-model="search.data.customerPhone" :disabled="!search.useCustomerPhone" class="bottom20 input-size">
						<el-checkbox label="客户手机号" slot="prepend" v-model="search.useCustomerPhone"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入商户订单号" v-model="search.data.tradeCode" :disabled="!search.useTradeCode" class="bottom20 input-size">
						<el-checkbox label="商户订单号" slot="prepend" v-model="search.useTradeCode"></el-checkbox>
					</el-input>

				</el-col>

				<el-col :span="12">
					<el-input placeholder="请输入订单流水号" v-model="search.data.tradeNo" :disabled="!search.useTradeNo" class="bottom20 input-size">
						<el-checkbox label="订单流水号" slot="prepend" v-model="search.useTradeNo"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="订单来源" v-model="search.useSource" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%" v-model="search.data.source" placeholder="请选择订单来源" :disabled="!search.useSource">
								<el-option label="请选择" value=""></el-option>
								<el-option label="PC端" value="1"></el-option>
								<el-option label="微信端" value="2"></el-option>
								<el-option label="安卓APP" value="3"></el-option>
								<el-option label="苹果APP" value="4"></el-option>
							</el-select>
						</el-col>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="结束时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
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
				<bc-statbox title="订单总金额" v-bind:part1="totalPayable" back="#20A0FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="订单总成本" v-bind:part1="totalCost" back="#58B7FF"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					{{searchStr}}
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-tabs v-model="tabIndex" @tab-click="handleClick">
						<el-tab-pane label="待付款订单" name="1">
							<el-table :data="table.items" border @selection-change="handleSelectionChange">
								<el-table-column type="selection" width="55">
								</el-table-column>
								<el-table-column prop="order_code" label="订单号">
								</el-table-column>
								<el-table-column prop="shopName" label="商户名称">
								</el-table-column>
								<el-table-column prop="customerPhone" label="客户手机号">
								</el-table-column>
								<el-table-column prop="totalPayable" label="订单总额">
								</el-table-column>
								<el-table-column prop="totalProductCost" label="订单总成本">
								</el-table-column>
								<el-table-column prop="source" label="订单来源">
								</el-table-column>
								<el-table-column prop="created_at" label="下单时间">
								</el-table-column>
								<el-table-column prop="status" label="订单状态">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
										<!-- <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button> -->
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="待发货订单" name="2">
							<el-table :data="table.items" border @selection-change="handleSelectionChange">
								<el-table-column prop="order_code" label="订单号">
								</el-table-column>
								<el-table-column prop="shopName" label="商户名称">
								</el-table-column>
								<el-table-column prop="theSameOrderNum" label="商户订单号">
								</el-table-column>
								<el-table-column prop="tradeNo" label="交易流水号">
								</el-table-column>
								<el-table-column prop="totalPayable" label="订单总额">
								</el-table-column>
								<el-table-column prop="totalProductCost" label="订单总成本">
								</el-table-column>
								<el-table-column prop="payTime" label="支付时间">
								</el-table-column>
								<el-table-column prop="status" label="订单状态">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
										<el-button size="small" @click="handleSend(scope.$index, scope.row)">发货</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="已发货订单" name="3">
							<el-table :data="table.items" border @selection-change="handleSelectionChange">
								<el-table-column prop="order_code" label="订单号">
								</el-table-column>
								<el-table-column prop="shopName" label="商户名称">
								</el-table-column>
								<el-table-column prop="theSameOrderNum" label="商户订单号">
								</el-table-column>
								<el-table-column prop="tradeNo" label="交易流水号">
								</el-table-column>
								<el-table-column prop="totalPayable" label="订单总额">
								</el-table-column>
								<el-table-column prop="totalProductCost" label="订单总成本">
								</el-table-column>
								<el-table-column prop="sendOutTime" label="发货时间">
								</el-table-column>
								<el-table-column prop="status" label="订单状态">
								</el-table-column>
								<el-table-column label="操作" width="150">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
										<!--<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button>-->
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="已收货订单" name="4">
							<el-table :data="table.items" border @selection-change="handleSelectionChange">
								<el-table-column prop="order_code" label="订单号">
								</el-table-column>
								<el-table-column prop="shopName" label="商户名称">
								</el-table-column>
								<el-table-column prop="theSameOrderNum" label="商户订单号">
								</el-table-column>
								<el-table-column prop="tradeNo" label="交易流水号">
								</el-table-column>
								<el-table-column prop="totalPayable" label="订单总额">
								</el-table-column>
								<el-table-column prop="totalProductCost" label="订单总成本">
								</el-table-column>
								<el-table-column prop="sendOutTime" label="收货时间">
								</el-table-column>
								<el-table-column prop="status" label="订单状态">
								</el-table-column>
								<el-table-column label="操作" width="150">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">查看</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="已关闭订单" name="5">
							<el-table :data="table.items" border @selection-change="handleSelectionChange">
								<el-table-column prop="order_code" label="订单号">
								</el-table-column>
								<el-table-column prop="shopName" label="商户名称">
								</el-table-column>
								<el-table-column prop="preferredContactPhone" label="客户手机号">
								</el-table-column>
								<el-table-column prop="totalPayable" label="订单总额">
								</el-table-column>
								<el-table-column prop="totalProductCost" label="订单总成本">
								</el-table-column>
								<el-table-column prop="source" label="订单来源">
								</el-table-column>
								<el-table-column prop="created_at" label="下单时间">
								</el-table-column>
								<el-table-column prop="status" label="订单状态">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
					</el-tabs>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="danger" size="small" v-if="tabIndex==1" :disabled="table.selections.length == 0" @click="cancelForm.visible=true">
									<i class="icon-minus icons"></i> 取消订单
								</el-button>
								<!-- <el-button type="info" size="small" @click="printtf()"><i class="icon-printer icons"></i> 打印</el-button> -->
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
						<el-col :span="12">
							<el-form-item label="订单号" :label-width="formLabelWidth">
								<el-input v-model="form.data.order_code" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="支付方式" :label-width="formLabelWidth">
								<el-input v-model="form.data.payType" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="快速公司" :label-width="formLabelWidth">
								<el-input v-model="form.data.logisticsName" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="姓名" :label-width="formLabelWidth">
								<el-input v-model="form.data.receiveName" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="所在省" :label-width="formLabelWidth">
								<el-input v-model="form.data.receiveProvince" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="所在区" :label-width="formLabelWidth">
								<el-input v-model="form.data.receiveDistrict" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="配送方式" :label-width="formLabelWidth">
								<el-input v-model="form.data.expressType" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="订单状态" :label-width="formLabelWidth">
								<el-input v-model="form.data.status" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="支付流水号" :label-width="formLabelWidth">
								<el-input v-model="form.data.tradeNo" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="订单总邮费" :label-width="formLabelWidth">
								<el-input v-model="form.data.deliveryPrice" :disabled="form.disabled"></el-input>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item label="客户手机号">
								<el-input v-model="form.data.preferredContactPhone" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="下单时间">
								<el-input v-model="form.data.orderTime" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="快速单号">
								<el-input v-model="form.data.expressCode" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="手机号">
								<el-input v-model="form.data.phone" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="所在市">
								<el-input v-model="form.data.receiveCity" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="详细地址">
								<el-input v-model="form.data.receiveDetailAddress" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="订单总额">
								<el-input v-model="form.data.totalPayable" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="订单总成本">
								<el-input v-model="form.data.totalProductCost" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="商户订单号">
								<el-input v-model="form.data.theSameOrderNum" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="客户备注">
								<el-input v-model="form.data.note" :disabled="form.disabled"></el-input>
							</el-form-item>
						</el-col>
						<template>
							<el-table :data="form.data.details" style="width: 100%">
								<el-table-column label="产品图片">
									<template slot-scope="props">
										<el-form label-position="left" inline class="demo-table-expand">
											<el-form-item>
												<img v-if="props.row.mainPic" style="width: 100%;" v-bind:src="props.row.mainPic" class="image">
											</el-form-item>
										</el-form>
									</template>
								</el-table-column>
								<el-table-column label="产品名称" prop="product_name">
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
						<div>
							<hr>
						</div>
						<el-form-item style="text-align: right">
							<el-button @click="form.visible=false">关闭</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				<el-dialog :title="form1.title" v-model="form1.visible" size="small">
					<el-form ref="form1" :model="form1.data" label-width="80px">
						<el-form-item label="快速公司">
							<el-select v-model="form1.data.logisticsNum" style="width: 100%" clearable placeholder="请选择快递公司">
								<el-option
							      v-for="item in expressCom"
							      :key="item.value"
							      :label="item.name"
							      :value="item.value">
							    </el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="快速单号">
							<el-input v-model="form1.data.expressCode" :disabled="form1.disabled"></el-input>
						</el-form-item>
						<el-form-item style="text-align: right">
							<el-button @click="form1.visible=false">取消</el-button>
							<el-button type="primary" @click="handleSendOut()">提交发货</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				<el-dialog :title="cancelForm.title" v-model="cancelForm.visible" size="small">
					<el-form ref="cancelForm" :model="cancelForm.data" label-width="80px">
						<el-form-item label="原因">
							<el-select v-model="cancelForm.data.cancel_reason" style="width: 100%">
								<el-option label="请选择" value=""></el-option>
								<el-option label="客户电话取消" value="客户电话取消"></el-option>
								<el-option label="缺货" value="缺货"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="备注">
							<el-input v-model="cancelForm.data.cancel_note"></el-input>
						</el-form-item>
						<el-form-item style="text-align: right">
							<el-button @click="cancelForm.visible=false">取消</el-button>
							<el-button type="primary" @click="handleBatchCancel()">取消订单</el-button>
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
				formLabelWidth: '120px',
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						address_id: '',
						customer_id: '',
						deliveryPrice: '',
						id: '',
						mainPic: '',
						note: '',
						order_code: '',
						shopName: '',
						preferredContactPhone: '',
						status: '',
						created_at: '',
						source: '',
						totalPayable: '',
						totalProductCost: ''
					}],
					selections: [],
				},
				tabIndex: '1',
				totalCost: 0,
				totalPayable: 0,
				uploadUrl: this.baseUrl + "admin/file/upload" + '&file=file',
				form1: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						id: '',
						logisticsNum: '',
						expressCode: ''
					}
				},
				cancelForm: {
					visible: false,
					title: '取消订单',
					data: {
						cancel_reason: '',
						cancel_note: ''
					}
				},
				form: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						id: '',
						receiveName: '',
						city: '',
						cityId: '',
						created_at: '',
						customerPhone: '',
						detailAddress: '',
						receiveDistrict: '',
						order_code: '',
						phone: '',
						receiveProvince: '',
						provinceId: '',
						theSameOrderNum: '',
						totalCost: '',
						totalPayable: '',
						tradeNo: '',
						details: [{
							actualUnitPrice: '',
							category_name: '',
							couponDiscount: '',
							id: '',
							mainPic: '',
							product_name: '',
							order_id: '',
							status: '',
							totalActualProductPrice: '',
							totalDeliveryCost: '',
							unitCost: '',
							unitOrdered: '',
							totalPrice: ''
						}]
					}
				},
				search: {
					visible: false,
					useCode: true,
					useShopName: true,
					useCustomerPhone: true,
					useTotalPayable: true,
					useTotalCost: true,
					useTradeCode: true,
					useTradeNo: true,
					useSource: true,
					useTimeType: true,
					useStartTime: true,
					useEndTime: true,
					useStatus: true,
					data: {
						code: '',
						shopName: '',
						customerPhone: '',
						totalPayable: '',
						totalCost: '',
						tradeCode: '',
						tradeNo: '',
						source: '',
						timeType: '',
						startTime: '',
						endTime: '',
						status: '',
					}
				},
				expressCom: [
		            {value:"shunfeng",name:"顺丰"},
		            {value:"zhongtong",name:"中通"},
		            {value:"yuantong",name:"圆通"},
		            {value:"shentong",name:"申通"},
		            {value:"ems",name:"EMS"},
		            {value:"bjemstckj",name:"北京EMS"},
		            {value:"huitongkuaidi",name:"汇通"},
		            {value:"yunda",name:"韵达"},
		            {value:"zhaijisong",name:"宅急送"},
		            {value:"tiantian",name:"天天"},
		            {value:"debangwuliu",name:"德邦"},
		            {value:"guotongkuaidi",name:"国通"},
		            {value:"zengyisudi",name:"增益"},
		            {value:"suer",name:"速尔"},
		            {value:"ztky",name:"中铁物流"},
		            {value:"zhongtiewuliu",name:"中铁快运"},
		            {value:"ganzhongnengda",name:"能达"},
		            {value:"youshuwuliu",name:"优速"},
		            {value:"quanfengkuaidi",name:"全峰"},
		            {value:"hkpost",name:"香港邮政(HongKong Post)"},
		            {value:"quanyikuaidi",name:"全一"},
		            {value:"rufengda",name:"如风达"},
		            {value:"lianhaowuliu",name:"联昊通"}
		        ],
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
			handleSend(id, row) {
				this.form1.title = "发货";
				this.form1.disabled = false;
				this.form1.data.id = row.id;
				this.form1.data.logisticsName = '';
				this.form1.data.expressCode = '';
				this.form1.visible = true;
			},
			handleBatchCancel() {
				var me = this;
				var ids = [];
				var selections = me.table.selections;
				for(var i = 0; i < selections.length; i++) {
					ids.push(selections[i].id);
				}

				if (!me.cancelForm.data.cancel_reason) {
					this.$alert('请选择取消订单的原因', '温馨提示', {
			          confirmButtonText: '确定',
			        });
			        return;
				}
 
				me.$confirm('此操作将取消选中的订单, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					me.$post('admin/order/batchCancel', {
							ids: JSON.stringify(ids),
							cancel_reason: me.cancelForm.data.cancel_reason,
							cancel_note: me.cancelForm.data.cancel_note
						}).then2(function(response) {
							if(response.data.error == 0) {
								me.$notify({
									title: '成功',
									message: '取消订单成功',
									type: 'success'
								});
								me.cancelForm.visible = false;
								me.handleSearch();
							} else {
								this.$notify.error({
						          title: '失败',
						          message: '取消订单失败'
						        });
							}
						}), function(response) {
							me.$notify.error({
								title: '错误',
								message: '删除失败'
							});
						};
				}).catch(() => {

				});
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "订单";
				me.form.visible = true;

				this.$get('admin/order/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.order;
						me.form.data.payType = me.$getOrderPayType(me.form.data.payType);
						switch(me.form.data.status) {
							case 1:
								me.form.data.status = "待付款";
								break;
							case 2:
								me.form.data.status = "待发货";
								break;
							case 3:
								me.form.data.status = "待收货";
								break;
							case 4:
								me.form.data.status = "待评价";
								break;
							case 5:
								me.form.data.status = "已评价";
								break;
							case 6:
								me.form.data.order.status = "已取消";
								break;
							case 7:
								me.form.data.status = "订单完成";
								break;
						}
						for(var i = 0; i < me.form.data.details.length; i++) {
							me.form.data.details[i].mainPic = me.$getAbsolutePath(me.form.data.details[i].mainPic);
							switch(me.form.data.details[i].status) {
								case 1:
									me.form.data.details[i].status = "正常";
									break;
								case 2:
									me.form.data.details[i].status = "申请退款";
									break;
								case 3:
									me.form.data.details[i].status = "不通过";
									break;
								case 4:
									me.form.data.details[i].status = "退款中";
									break;
								case 5:
									me.form.data.details[i].status = "退款成功";
									break;
								case 6:
									me.form.data.details[i].status = "已退货";
									break;
								case 7:
									me.form.data.details[i].status = "已评价";
									break;
								case 8:
									me.form.data.details[i].status = "退款失败";
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
			handleRefuse() {
				var me = this;
				me.form.visible = false;
				this.$post('admin/shop/refuse', {
						id: me.form.data.id
					}).then2(function(response) {
						if(response.data.error == 0) {
							me.$notify({
								title: '成功',
								message: '拒绝成功',
								type: 'success'
							});
							me.handleSearch();
						}
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '操作失败',
						});
					};
			},
			handleSendOut() {
				var me = this;
				
				if (!me.form1.data.logisticsNum) {
					this.$alert('请选择快递公司', '提示', {
			          confirmButtonText: '确定',
			        });
			        return;
				}
				if (!me.form1.data.expressCode) {
					this.$alert('请填写快递编号', '提示', {
			          confirmButtonText: '确定',
			        });
			        return;
				}

				var logisticsName = "";
				for (var i = 0; me.expressCom.length; i++) {
					if (me.expressCom[i].value == me.form1.data.logisticsNum) {
						logisticsName = me.expressCom[i].name;
						break;
					}
				}

				this.$post('admin/order/sendOut', {
						id: me.form1.data.id,
						deliveryCompany: logisticsName,
						deliveryCompanyNum: me.form1.data.logisticsNum,
						trackingNumber: me.form1.data.expressCode
					}).then2(function(response) {
						if(response.data.error == 0) {
							me.$notify({
								title: '成功',
								message: '通过成功',
								type: 'success'
							});
							me.form1.visible = false;
							me.handleSearch();
						}
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '操作失败',
						});
					};
			},
			handleDelete(id, row) {

			},
			handleDeleteSelections() {

			},
			handleSubmit() {

			},
			handleClick(tab, event) {
				this.tabIndex = tab.$options.propsData.name;
				this.table.pageIndex = 1;
				this.table.pageCount = 25;
				this.handleSearch();
			},
			getSearchKey(key) {
				var me = this;
				if(this.search.useCode && this.search.data.code) {
					key.code = this.search.data.code;
					me.searchStr += ' 订单号： ' + key.code ;
					if(key.code != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useShopName && this.search.data.shopName) {
					key.shopName = this.search.data.shopName;
					me.searchStr += ' 商户名称: ' + key.shopName;
					if(key.shopName != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useTradeCode && this.search.data.tradeCode) {
					key.tradeCode = this.search.data.tradeCode;
					me.searchStr += ' 客户订单号： ' + key.tradeCode;
					if(key.tradeCode != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useTradeNo && this.search.data.tradeNo) {
					key.tradeNo = this.search.data.tradeNo;
					me.searchStr += ' 订单流水号： ' + key.tradeNo;
					if(key.tradeNo != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useSource && this.search.data.source) {
					key.source = this.search.data.source;
					var temp="";
					switch(parseInt(key.source))
					{
						case 1:
							temp = "pc端";
							break;
						case 2:
							temp = "微信端";
							break;
						case 3:
							temp = "安卓APP";
							break;
						case 4:
							temp = "苹果APP";
							break;
					}
					me.searchStr += ' 订单来源： ' + temp;
					if(key.source != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useTimeType && this.search.data.timeType) {
					key.timeType = this.search.data.timeType;
					var temp="";
					switch(parseInt(key.timeType)){
						case 0:
							temp = "申请时间";
							break;
						case 1:
							temp = "退款时间";
							break;
						case 2:
							temp = "处理时间";
							break;
						case 3:
							temp = "下单时间";
							break;
						case 4:
							temp = "支付时间";
							break;
						case 5:
							temp ="发货时间";
							break;
						case 6:
							temp ="收货时间";
							break;
						case 7:
							temp = "关闭时间";
							break;
					}
					me.searchStr += ' 时间类型： ' +temp;
					if(key.timeType != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$formatDate(this.search.data.startTime);
					me.searchStr += ' 开始时间： ' + key.startTime ;
					if(key.startTime != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$formatDate(this.search.data.endTime);
					me.searchStr += ' 结束时间： ' + key.endTime ;
					if(key.endTime != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useStatus && this.search.data.status) {
					key.status = this.search.data.status;
					var temp;
					switch(parseInt(key.status)){
						case 1:
							temp = "正常";
							break;
						case 2:
							temp = "申请退款";
							break;
						case 3:
							temp = "退货退款不通过";
							break;
						case 4:
							temp ="退款中";
							break;
						case 5:
							temp ="退款成功";
							break;
						case 6:
							temp ="已退货";
							break;
						case 7:
							temp ="已评价";
							break;
						case 8:
							temp ="退款失败";
							break;
						default:
							temp = key.status
					}
					me.searchStr += ' 订单状态： ' + temp ;
					if(key.status != '') {
						me.searchStr += ';';
					}
				}
				key.status = this.tabIndex;

				return key;
			},
			handleSearch() {
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				key = me.getSearchKey(key);
				
				this.$post("admin/order/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.totalCost = response.data.totalCost;
						me.totalPayable = response.data.totalPayable;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							var item = me.table.items[i];
							item.status = me.$getOrderStatus(item.status);
							item.source = me.$getOrderSource(item.source);
						}
					}).catch(function(err) { 
						console.log(err);
					});
			},
			handleExport() {
				var key = {};
				key = this.getSearchKey(key);
				this.$post("admin/excel/exportOrders", key)
					.then2(function(response) {
						window.location.href = response.data.path;
					}).catch(function(err) { 
						console.log(err);
					});
			},
		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
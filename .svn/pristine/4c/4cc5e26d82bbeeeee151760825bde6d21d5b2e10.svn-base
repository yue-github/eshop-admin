<template scoped>

	<div>
		<!--查询弹出框-->
		<el-dialog title="查询销售汇总信息" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: left">
							<el-checkbox label="开始支付时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15" style="text-align: center">
							<el-date-picker style="margin-top: 5px; margin-left: 10px;" v-model="search.data.startTime" type="date" format="yyyy-MM-dd" placeholder="选择开始日期" :disabled="!search.useStartTime">
							</el-date-picker>
						</el-col>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: left">
							<el-checkbox label="结束支付时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="15" style="text-align: center">
							<el-date-picker style="margin-top: 5px; margin-left: 10px;" v-model="search.data.endTime" type="date" format="yyyy-MM-dd" placeholder="选择结束日期" :disabled="!search.useEndTime">
							</el-date-picker>
						</el-col>
					</el-row>

				</el-col>
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="支付方式" v-model="search.usePayType" style="margin-top: 10px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="margin-top: 5px; margin-left: 10px;width: 100%;" multiple v-model="search.data.payType" placeholder="请选择支付方式" :disabled="!search.usePayType">
								<el-option label="微信支付" value="1"></el-option>
								<el-option label="支付宝支付" value="2"></el-option>
								<el-option label="银联支付" value="3"></el-option>
								<el-option label="团购卡支付" value="5"></el-option>
								<el-option label="积分兑换" value="6"></el-option>
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
			<el-col :span="4">
				<bc-statbox title="总销售额" v-bind:part1="totalPayable" back="#58B7FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="总退款金额" v-bind:part1="totalRefundCash" back="#58B7FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="总手续费" v-bind:part1="totalPayRateSum" back="#13CE66"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="总税额" v-bind:part1="totalTaxCost" back="#1D8CE0"></bc-statbox>
			</el-col>
			<!-- <el-col :span="4">
				<bc-statbox title="总产品价格" v-bind:part1="totalActualProductPrice" back="#20A0FF"></bc-statbox>
			</el-col> -->
			<el-col :span="4">
				<bc-statbox title="总成本" v-bind:part1="totalCost" back="#58B7FF"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table v-loading="loading" :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="payType" label="支付方式">
						</el-table-column>
						<el-table-column prop="totalPayable" label="销售金额">
						</el-table-column>
						<el-table-column prop="totalCost" label="销售成本">
						</el-table-column>
						<el-table-column prop="totalRefundCash" label="退款金额">
						</el-table-column>
						<el-table-column prop="payRateSum" label="手续费">
						</el-table-column>
						<el-table-column prop="totalTaxCost" label="税额">
						</el-table-column>
						<el-table-column prop="balance" label="划账金额">
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 禁用
								</el-button>
								<el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<!-- <el-button type="info" size="small" @click="printtf()"><i class="icon-printer icons"></i> 打印</el-button> -->
								<!-- <el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button> -->
							</el-button-group>
						</el-col>
						<el-col :span="16" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
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
							<el-input v-model="form.data.mobilePhone" placeholder="请输入手机号码"></el-input>
						</el-form-item>
						<el-form-item label="用户简介">
							<el-input v-model="form.data.note" placeholder="请输入用户简介"></el-input>
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
				<el-col :span='24' style="text-align: center">
					销售汇总表查询
				</el-col>
			</el-row>
			<el-row :gutter="20" style="margin-bottom: 20px;">
				<el-col :span='12' style="text-align: left; margin-top: 20px;">
					<div style="position: relative;left: 0px;display: inline-block;">供应商：</div>
					<div style="position: absolute;left: 300px;display: inline-block;">时间段：</div>
					<div style="position: absolute;left: 500px;display: inline-block;">订单状态：(未发货/在途/已收货/退货)：</div>
					<div style="position: absolute;left: 950px;display: inline-block;">单位：元</div>
				</el-col>
			</el-row>
			<el-row :gutter="20">
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
							<td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.supplierName}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.receiveTime}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.taxRateSum}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.totalPayable}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.refundCash}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;"></td>
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
							<td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">制表人：</td>
							<td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">审核人：</td>
						</tr>
					</table>
				</el-col>
			</el-row>
		</div>
	</div>
</template>
<style>
	.el-date-editor.el-input {
		width: 100%;
	}
</style>
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
						payType: '',
						totalPayable: '',
						totalCost: '',
						totalRefundCash: '',
						payRateSum: '',
						totalTaxCost: '',
						balance: ''
					}, ],
					selections: [],
				},
				loading: false,
				totalCost: 0,
				totalActualProductPrice: 0,
				totalTaxCost: 0,
				totalPayRateSum: 0,
				totalRefundCash: 0,
				totalPayable: 0,
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
					usePayType: true,
					useStartTime: true,
					useEndTime: true,
					data: {
						payType: [],
						startTime: '',
						endTime: '',
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
			getSearchSection() {
				var me = this;
				var key = {};
				me.searchStr = "搜索";
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;

				if(this.search.usePayType && this.search.data.payType.length > 0) {
					key.payType = JSON.stringify(this.search.data.payType);
					me.searchStr += ' 支付方式: ' + this.$getOrderPayTypes(this.search.data.payType);
					if(key.searchStr!=""){
						me.searchStr += "；"
					}
				}
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$formatDate(this.search.data.startTime);
					me.searchStr += ' 开始支付时间: ' + key.startTime;
					if(key.startTime!=""){
						me.searchStr += "；"
					}
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$formatDate(this.search.data.endTime);
					me.searchStr += ' 结束支付时间: ' + key.endTime;
					if(key.searchStr!=""){
						me.searchStr += "；"
					}
				}
				return key;
			},
			handleSearch() {
				var me = this;
				me.searchStr = '搜索';
				var key = me.getSearchSection();

				this.$get("admin/order/orderPayList", key).then2(function(response) {
					for(var i = 0; i < response.data.data.length; i++) {
						response.data.data[i].payType = me.$getOrderPayType(response.data.data[i].payType);
					}
					me.table.items = response.data.data;
					me.table.total = response.data.totalRow;
					me.totalProductCost = response.data.totalProductCost;
					me.totalActualProductPrice = response.data.totalActualProductPrice;
					me.totalTaxCost = response.data.totalTaxCost;
					me.totalPayRateSum = response.data.totalPayRateSum;
					me.totalRefundCash = response.data.totalRefundCash;
					me.totalPayable = response.data.totalPayable;
					me.totalCost = response.data.totalCost;
					me.search.visible = false;
				}).catch(function(err) { 
					console.log(err);
				});
			},
			handleExport() {
				var key = this.getSearchSection();
				this.$get('admin/order/exportOrderPayList', key).then2(function(response) {
					if(response.data.error > 0) {
						alert(response.errmsg);
					} else {
						window.open(response.data.path);
					}
				}).catch(function(err) {
					console.log(err)
				});
			},
			//打印功能
			printtf: function() {
				this.bundprint('pdf-wrap');
			},
		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
<style>
	.input-size {
		width: 87%
	}
</style>
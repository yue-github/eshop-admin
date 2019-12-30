<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询商品种类销量" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="开始时间" v-model="search.useStartDatetime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							 <el-date-picker v-model="search.data.startDatetime" type="datetime" placeholder="选择日期时间" default-time="12:00:00"></el-date-picker>
						</el-col>
					</el-row>
					<el-input placeholder="请输入名称关键字" v-model="search.data.customerName" :disabled="!search.useCustomerName" class="bottom20 input-size">
						<el-checkbox label="分类名称" slot="prepend" v-model="search.useCustomerName"></el-checkbox>
					</el-input>
				</el-col>
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="结束时间" v-model="search.useEndDatetime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							 <el-date-picker v-model="search.data.endDatetime" type="datetime" placeholder="选择日期时间" default-time="12:00:00"></el-date-picker>
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
	    	<el-col :span="4"><bc-statbox title="总销售数量" v-bind:part1="totalSalesVolume" back="#409EFF"></bc-statbox></el-col>
	    	<el-col :span="4"><bc-statbox title="总销售金额" v-bind:part1="totalPrice" back="#409EFF"></bc-statbox></el-col>
	    </el-row>
		<el-row :gutter="20">
			<el-col :span="24">
					<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column prop="cname" label="分类名称">
						</el-table-column>
						<el-table-column prop="orderTime" label="销售时间">
						</el-table-column>
						<el-table-column prop="salesVolume" label="销售数量">
						</el-table-column>
						<el-table-column prop="salesPrice" label="销售总额">
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="24" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
			</el-col>
		</el-row>
		<div id="pdf-wrap" style="display:none;">
			<el-row :gutter="20" style="margin-bottom: 20px">
				<el-col :span='24'>
					<div style="width: 100%;text-align: center;">提现记录</div>
				</el-col>
			</el-row>
			<el-row :gutter="20">
				<el-col :span='24'>
					<table width="100%" border="1" cellspacing="0" cellpadding="0">
						<tr width="100%">
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">用户名称</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">用户昵称</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">用户手机</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">转账账号</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">提现类型</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 150px">金额</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">状态</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">申请时间</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">到账时间</td>
						</tr>
						<tr width="100%" v-for="(item,index) in table.items">
							<td style="padding: 5px;width:15%;font-size:12px;text-align:center;">{{item.customerName}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.nickName}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.mobilePhone}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.account}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.accountType}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.money}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.status}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.created_at}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.arriveTime}}</td>
						</tr>
						<tr width="100%">
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">合计</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 150px">{{totalMoney}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
						</tr>
						<tr width="100%">
							<td colspan="3" style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
							<td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
						</tr>
						<tr width="100%">
							<td colspan="3" style="padding: 5px;font-size:12px;text-align:left;">制表人：</td>
							<td colspan="6" style="padding: 5px;font-size:12px;text-align:left;">审核人：</td>
						</tr>
					</table>
				</el-col>
			</el-row>
		</div>

	</div>
</template>

<script>
	module.exports = {
		data: function() {
			return {
				searchStr: '',
				totalPrice: 0,
				totalSalesVolume: 0,
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						customerName: '测试1',
						nickName: 'haha',
						mobilePhone: '18000000000',
						account: '1234560000',
						accountType: '支付宝',
						money: '150000',
						status: '通过申请',
						created_at: '2018-01-31',
						arriveTime: '2018-02-02',
					}, ],
					selections: [],
				},
				totalMoney: 0,
				form: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						customerName: '',
						weixinAccount: '',
						money: '',
						status: '',
						arriveTime: '',
						note: ''
					}
				},
				CommodityCategoryVisitNum:9,
				search: {
					visible: false,
					useCustomerName: true,
					useStartDatetime: true,
					useEndDatetime: true,
					data: {
						customerName: '',
						startDatetime: '',
						endDatetime: '',
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
				this.form.title = "手续费信息";
				this.form.disabled = false;
				this.form.data.payType = '';
				this.form.data.enable = '';
				this.form.data.rate = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "查看记录";
				me.form.visible = true;

				this.$get('admin/withdrawCash/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(me.form.data.accountType == "支付宝") {
							me.form.data.accountType = 1;
						} else if(me.form.data.accountType == "微信支付") {
							me.form.data.accountType = 2;
						} else if(me.form.data.accountType == "银行卡") {
							me.form.data.accountType = 3;
						}
						if(me.form.data.status == 0) {
							me.form.data.status = "提交申请";
						} else if(me.form.data.status == 1) {
							me.form.data.status = "不启用";
						} else if(me.form.data.status == 2) {
							me.form.data.status = "不启用";
						} else if(me.form.data.status == 3) {
							me.form.data.status = "已转账";
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
			handleSubmit() {

			},
			handleSearch() {
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = "搜索";
				if(this.search.useCustomerName && this.search.data.customerName) {
					key.customerName = this.search.data.customerName;
					me.searchStr += ' 分类名称： ' + key.customerName ;
					if(key.customerName != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useStartDatetime && this.search.data.startDatetime) {
					key.startDatetime = this.$filter(this.search.data.startDatetime,'yyyy-mm-dd','1');
					me.searchStr += ' 开始时间： ' + key.startDatetime;
					if(key.startDatetime){
						me.searchStr+=";";
					}
				}
				if(this.search.useEndDatetime && this.search.data.endDatetime) {
					key.endDatetime = this.$filter(this.search.data.endDatetime,'yyyy-mm-dd','1');
					me.searchStr += ' 结束时间： ' + key.endDatetime ;
				}
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useCustomerName && this.search.data.customerName) {
					key.cname = this.search.data.customerName;
				}
				if(this.search.useStartDatetime && this.search.data.startDatetime) {
					key.startDatetime = this.search.data.startDatetime;
				}
				if(this.search.useEndDatetime && this.search.data.endDatetime) {
					key.endDatetime = this.search.data.endDatetime;
				}
				
				var me = this;
				// alert(JSON.stringify(key));
				this.$get("admin/salesVolume/categoryVolume", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.totalSalesVolume = response.data.totalSalesVolume;
						me.totalPrice = response.data.totalPrice;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							me.CommodityCategoryVisitNum = me.table.items[i].salesVolume + me.CommodityCategoryVisitNum;
						}
					}).catch(function(err) { 
						console.log(err);
					});
			},
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
<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询充值记录" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入商户订单号" v-model="search.data.tradeNo" :disabled="!search.useTradeNo" class="bottom20 input-size">
						<el-checkbox label="商户订单号" slot="prepend" v-model="search.useTradeNo"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入会员名称" v-model="search.data.customerName" :disabled="!search.useCustomerName" class="bottom20 input-size">
						<el-checkbox label="会员名称" slot="prepend" v-model="search.useCustomerName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入会员手机号" v-model="search.data.customerPhone" :disabled="!search.useCustomerPhone" class="bottom20 input-size">
						<el-checkbox label="会员手机" slot="prepend" v-model="search.useCustomerPhone"></el-checkbox>
					</el-input>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="充值类型" v-model="search.useEvent" style="margin-top: 5px;"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%;" v-model="search.data.event" placeholder="请选择充值类型" :disabled="!search.useEvent">
								<el-option label="微信充值" value="1"></el-option>
								<el-option label="支付宝充值" value="2"></el-option>
								<el-option label="银联充值" value="3"></el-option>
								<el-option label="钱包支付" value="4"></el-option>
							</el-select>
						</el-col>
					</el-row>

				</el-col>

				<el-col :span="12">
					<el-input placeholder="请输入充值金额大于" v-model="search.data.moneyMoreThan" :disabled="!search.useMoneyMoreThan" class="bottom20 input-size">
						<el-checkbox label="充值金额大于" slot="prepend" v-model="search.useMoneyMoreThan"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入充值金额小于" v-model="search.data.moneyLessThan" :disabled="!search.useMoneyLessThan" class="bottom20 input-size">
						<el-checkbox label="充值金额小于" slot="prepend" v-model="search.useMoneyLessThan"></el-checkbox>
					</el-input>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center;">
							<el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.startTime" type="date" placeholder="选择日期" :disabled="!search.useStartTime"></el-date-picker>
						</el-col>
					</el-row>

					<!--<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="结束时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.endTime" type="date" placeholder="选择日期" :disabled="!search.useEndTime"></el-date-picker>
						</el-col>
					</el-row>-->
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>

		<el-row :gutter="20" class="bottom20">
			<el-col :span="4">
				<bc-statbox title="总充值金额" v-bind:part1="totalMoney" back="#58B7FF"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20">
			<!--<el-col :span="4">
        <bc-panel title="查询充值记录" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>

          <el-input placeholder="请输入商户订单号" v-model="search.data.tradeNo" :disabled="!search.useTradeNo" class="bottom20">
            <el-checkbox label="商户订单号" slot="prepend" v-model="search.useTradeNo"></el-checkbox>
          </el-input>

          <el-input placeholder="请输入会员名称" v-model="search.data.customerName" :disabled="!search.useCustomerName" class="bottom20">
            <el-checkbox label="会员名称" slot="prepend" v-model="search.useCustomerName"></el-checkbox>
          </el-input>

          <el-input placeholder="请输入会员手机号" v-model="search.data.customerPhone" :disabled="!search.useCustomerPhone" class="bottom20">
            <el-checkbox label="会员手机" slot="prepend" v-model="search.useCustomerPhone"></el-checkbox>
          </el-input>

          <el-row class="bottom20">
            <el-col :span="8" style="text-align: left">
              <el-checkbox label="充值类型" v-model="search.useEvent" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select v-model="search.data.event" placeholder="请选择充值类型" :disabled="!search.useEvent">
                <el-option label="支付宝充值" value="3"></el-option>
                <el-option label="微信充值" value="4"></el-option>
                <el-option label="银联充值" value="6"></el-option>
              </el-select>
            </el-col>
          </el-row>

          <el-input placeholder="请输入充值金额大于" v-model="search.data.moneyMoreThan" :disabled="!search.useMoneyMoreThan" class="bottom20">
            <el-checkbox label="充值金额大于" slot="prepend" v-model="search.useMoneyMoreThan"></el-checkbox>
          </el-input>

          <el-input placeholder="请输入充值金额小于" v-model="search.data.moneyLessThan" :disabled="!search.useMoneyLessThan" class="bottom20">
            <el-checkbox label="充值金额小于" slot="prepend" v-model="search.useMoneyLessThan"></el-checkbox>
          </el-input>

          <el-row class="bottom20">
            <el-col :span="8" style="text-align: left">
              <el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
	            <el-date-picker v-model="search.data.startTime"  type="date" placeholder="选择日期":disabled="!search.useStartTime"></el-date-picker>
           </el-col>
           </el-row>
           
          <el-row class="bottom20">
            <el-col :span="8" style="text-align: left">
              <el-checkbox label="结束时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
	            <el-date-picker v-model="search.data.endTime"  type="date" placeholder="选择日期":disabled="!search.useEndTime"></el-date-picker>
           </el-col>
           </el-row>
           
        </bc-panel>
      </el-col>-->
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="tradeNo" label="商户订单号" >
						</el-table-column>
						<el-table-column prop="customerName" label="会员名称" width="100">
						</el-table-column>
						<el-table-column prop="customerPhone" label="会员手机" width="150">
						</el-table-column>
						<el-table-column prop="event" label="充值类型" width="100">
						</el-table-column>
						<el-table-column prop="money" label="金额" width="150">
						</el-table-column>
						<el-table-column prop="created_at" label="充值时间">
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">查看</el-button>
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="24" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="small">
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="会员名称">
							<el-input v-model="form.data.customerName" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="会员手机">
							<el-input v-model="form.data.customerPhone" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="商户订单号">
							<el-input v-model="form.data.tradeNo" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="充值金额(元)">
							<el-input v-model="form.data.money" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="充值类型">
							<el-input v-model="form.data.event" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="充值时间">
							<el-input v-model="form.data.created_at" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="充值说明">
							<el-input v-model="form.data.note" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item style="text-align: right">

							<el-button @click="form.visible=false">取消</el-button>
							<!--<el-button type="primary" @click="handleSubmit()">保存</el-button>-->
						</el-form-item>
					</el-form>
				</el-dialog>
			</el-col>
		</el-row>
		<div id="pdf-wrap" style="display:none;">
			<el-row :gutter="20" style="margin-bottom: 20px">
				<el-col :span='24'>
					<div style="width: 100%;text-align: center;">充值管理记录</div>
				</el-col>
			</el-row>
			<el-row :gutter="20">
				<el-col :span='24'>
					<table width="100%" border="1" cellspacing="0" cellpadding="0">
						<tr width="100%">
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">商户订单号</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">会员名称</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">会员手机</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">充值类型</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">金额</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 150px">充值时间</td>
						</tr>
						<tr width="100%" v-for="(item,index) in table.items">
							<td style="padding: 5px;width:15%;font-size:12px;text-align:center;">{{item.tradeNo}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.customerName}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.customerPhone}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.event}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.money}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{item.created_at}}</td>
						</tr>
						<tr width="100%">
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">合计</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalMoney}}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
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
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						tradeNo: '1',
						customerName: 'haha',
						customerPhone: '13156685256',
						event: '微信充值',
						money: '5000',
						created_at: '2018-02-05'
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
						customerPhone: '',
						money: '',
						status: '',
						tradeNo: '',
						note: '',
						source: '',
						created_at: ''
					}
				},
				search: {
					visible: false,
					useTradeNo: true,
					useCustomerName: true,
					useCustomerPhone: true,
					useEvent: true,
					useMoneyMoreThan: true,
					useMoneyLessThan: true,
					useStartTime: true,
					useEndTime: true,
					data: {
						tradeNo: '',
						customerName: '',
						customerPhone: '',
						event: '',
						moneyMoreThan: '',
						moneyLessThan: '',
						startTime: '',
						endTime: ''
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
			},
			handleCurrentChange(val) {
				this.table.pageIndex = val;
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
				var me = this;
				me.form.title = "查看记录";
				me.form.visible = true;

				this.$get('admin/wallet/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(me.form.data.event == 1) {
							me.form.data.event = "微信支付";
						} else if(me.form.data.event == 2) {
							me.form.data.event = "支付宝";
						} else if(me.form.data.event == 3) {
							me.form.data.event = "银联支付";
						} else if(me.form.data.event == 4) {
							me.form.data.event = "钱包支付";
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
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;

				if(this.search.useTradeNo && this.search.data.tradeNo) {
					key.tradeNo = this.search.data.tradeNo;
					me.searchStr += ' 客户订单号： ' + key.tradeNo;
					if(key.tradeNo != '') {
						me.searchStr += ';';
					}
				}

				if(this.search.useCustomerName && this.search.data.customerName) {
					key.customerName = this.search.data.customerName;
					me.searchStr += ' 会员名称： ' + key.customerName;
					if(key.customerName != '') {
						me.searchStr += ';';
					}
				}

				if(this.search.useCustomerPhone && this.search.data.customerPhone) {
					key.customerPhone = this.search.data.customerPhone;
					me.searchStr += ' 会员手机： ' + key.customerPhone;
					if(key.customerPhone != '') {
						me.searchStr += ';';
					}
				}

				if(this.search.useEvent && this.search.data.event) {
					key.event = this.search.data.event;
					var temp;
					switch(parseInt(key.event)){
						case 1:
							temp = "微信充值";
							break;
						case 2:
							temp ="支付宝充值"
							break;
						case 3:
							temp ="银联充值"
							break;
						case 4:
							temp ="钱包支付";
							break;
					}
					me.searchStr += ' 充值类型： ' + temp;
					if(key.event != '') {
						me.searchStr += ';';
					}
				}

				if(this.search.useMoneyMoreThan && this.search.data.moneyMoreThan) {
					key.moneyMoreThan = this.search.data.moneyMoreThan;
					me.searchStr += ' 充值金额大于： ' + key.moneyMoreThan;
					if(key.moneyMoreThan != '') {
						me.searchStr += ';';
					}
				}

				if(this.search.useMoneyLessThan && this.search.data.moneyLessThan) {
					key.moneyLessThan = this.search.data.moneyLessThan;
					me.searchStr += ' 充值金额小于： ' + key.moneyLessThan;
					if(key.moneyLessThan != '') {
						me.searchStr += ';';
					}
				}

				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$filter(this.search.data.startTime, 'yyyy-mm-dd', '1');
					me.searchStr += ' 开始时间： ' + key.startTime;
					if(key.startTime != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$filter(this.search.data.endTime, 'yyyy-mm-dd', '1');
					me.searchStr += '8' + key.endTime;
					if(key.endTime != '') {
						me.searchStr += ';';
					}
				}

				var me = this;
				this.$get("admin/wallet/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.totalProductCost = response.data.totalMoney;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].event == 1) {
								me.table.items[i].event = "微信支付";
							} else if(me.table.items[i].event == 2) {
								me.table.items[i].event = "支付宝";
							} else if(me.table.items[i].event == 3) {
								me.table.items[i].event = "银联支付";
							} else if(me.table.items[i].event == 4) {
								me.table.items[i].event = "钱包支付";
							}
						}

					}).catch(function(err) { 
						console.log(err);
					});
			},

			handleExport() {

				this.$get('admin/wallet/exportRecharge')
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							window.open(response.path);
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
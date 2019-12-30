<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询提现记录" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入名称关键字" v-model="search.data.customerName" :disabled="!search.useCustomerName" class="bottom20 input-size">
						<el-checkbox label="用户名称" slot="prepend" v-model="search.useCustomerName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入昵称关键字" v-model="search.data.nickName" :disabled="!search.useNickName" class="bottom20 input-size">
						<el-checkbox label="用户昵称" slot="prepend" v-model="search.useNickName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入手机号码" v-model="search.data.mobilePhone" :disabled="!search.useMobilePhone" class="bottom20 input-size">
						<el-checkbox label="用户手机" slot="prepend" v-model="search.useMobilePhone"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入转账账号关键字" v-model="search.data.weixinAccount" :disabled="!search.useAccount" class="bottom20 input-size">
						<el-checkbox label="转账账号" slot="prepend" v-model="search.useAccount"></el-checkbox>
					</el-input>

					<!--<el-input placeholder="请输入金额大于" v-model="search.data.moneyMoreThan" :disabled="!search.useMoneyMoreThan" class="bottom20 input-size">
						<el-checkbox label="金额大于" slot="prepend" v-model="search.useMoneyMoreThan"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入金额小于" v-model="search.data.moneyLessThan" :disabled="!search.useMoneyLessThan" class="bottom20 input-size">
						<el-checkbox label="金额小于" slot="prepend" v-model="search.useMoneyLessThan"></el-checkbox>
					</el-input>-->

				</el-col>
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="提现类型" v-model="search.useAccountType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%;" v-model="search.data.accountType" placeholder="请选择类型" :disabled="!search.useAccountType">
								<el-option label="支付宝" value="1"></el-option>
								<el-option label="微信账号" value="2"></el-option>
								<el-option label="银行卡" value="3"></el-option>
							</el-select>
						</el-col>
					</el-row>

					<!--<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="时间类型" v-model="search.useTimeType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%;" v-model="search.data.timeType" placeholder="" :disabled="!search.useTimeType">
								<el-option label="申请时间" value="1"></el-option>
								<el-option label="到账时间" value="9"></el-option>
							</el-select>
						</el-col>
					</el-row>-->

					<!--<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="申请时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="到账时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>-->

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="状态" v-model="search.useStatus" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%;" v-model="search.data.status" placeholder="请选择类型" :disabled="!search.useStatus">
								<el-option label="请选择状态类型" value=""></el-option>
								<el-option label="提交申请" value="0"></el-option>
								<el-option label="通过申请" value="1"></el-option>
								<el-option label="拒绝申请" value="2"></el-option>
								<el-option label="已转账" value="3"></el-option>
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
				<bc-statbox title="提现总金额" v-bind:part1="totalMoney" back="#58B7FF"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20">
			<!-- <el-col :span="4">
        <bc-panel title="查询提现记录" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
          	<el-button type="primary" size="small" icon="search" @click="search.visible = true">高级查询</el-button>
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>
          <el-input placeholder="请输入名称关键字" v-model="search.data.customerName" :disabled="!search.useCustomerName" class="bottom20">
            <el-checkbox label="用户名称" slot="prepend" v-model="search.useCustomerName"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入昵称关键字" v-model="search.data.nickName" :disabled="!search.useNickName" class="bottom20">
            <el-checkbox label="用户昵称" slot="prepend" v-model="search.useNickName"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入手机号码" v-model="search.data.mobilePhone" :disabled="!search.useMobilePhone" class="bottom20">
            <el-checkbox label="用户手机" slot="prepend" v-model="search.useMobilePhone"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入转账账号关键字" v-model="search.data.account" :disabled="!search.useAccount" class="bottom20">
            <el-checkbox label="转账账号" slot="prepend" v-model="search.useAccount"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="提现类型" v-model="search.useAccountType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.accountType" placeholder="请选择类型" :disabled="!search.useAccountType">
                <el-option label="支付宝" value="1"></el-option>
                <el-option label="微信账号" value="2"></el-option>
                <el-option label="银行卡" value="3"></el-option>
              </el-select>
            </el-col>
          </el-row>
         <el-input placeholder="请输入金额大于" v-model="search.data.moneyMoreThan" :disabled="!search.useMoneyMoreThan" class="bottom20">
            <el-checkbox label="金额大于" slot="prepend" v-model="search.useMoneyMoreThan"></el-checkbox>
          </el-input>
         <el-input placeholder="请输入金额小于" v-model="search.data.moneyLessThan" :disabled="!search.useMoneyLessThan" class="bottom20">
            <el-checkbox label="金额小于" slot="prepend" v-model="search.useMoneyLessThan"></el-checkbox>
          </el-input>
           <el-row class="bottom20">
          <el-col :span="7" style="text-align: center">
            <el-checkbox label="状态" v-model="search.useStatus" style="margin-top: 5px"></el-checkbox>
          </el-col>
          <el-col :span="17">
            <el-select v-model="search.data.status" placeholder="请选择类型" :disabled="!search.useStatus">
              <el-option label="请选择状态类型" value=""></el-option>
              <el-option label="提交申请" value="0"></el-option>
              <el-option label="通过申请" value="1"></el-option>
              <el-option label="拒绝申请" value="2"></el-option>
              <el-option label="已转账" value="3"></el-option>
            </el-select>
          </el-col>
        </el-row>
           <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="时间类型" v-model="search.useTimeType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.timeType" placeholder="" :disabled="!search.useTimeType">
                <el-option label="申请时间" value="1"></el-option>
                <el-option label="到账时间" value="9"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="开始" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="结束" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
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
						<el-table-column prop="customerName" label="用户名称">
						</el-table-column>
						<el-table-column prop="nickName" label="用户昵称">
						</el-table-column>
						<el-table-column prop="mobilePhone" label="用户手机">
						</el-table-column>
						<el-table-column prop="weixinAccount" label="转账账号">
						</el-table-column>
						<el-table-column prop="accountType" label="提现类型">
						</el-table-column>
						<el-table-column prop="money" label="金额">
						</el-table-column>
						<el-table-column prop="status" label="状态">
						</el-table-column>
						<el-table-column prop="created_at" label="申请时间">
						</el-table-column>
						<el-table-column prop="arriveTime" label="到账时间">
						</el-table-column>
						<el-table-column label="查看">
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
				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="会员名称">
							<el-input v-model="form.data.customerName" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="微信账号">
							<el-input v-model="form.data.weixinAccount" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="提现金额">
							<el-input v-model="form.data.money" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="状态">
							<el-input v-model="form.data.status" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="到账时间">
							<el-input v-model="form.data.arriveTime" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="提现说明">
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
				search: {
					visible: false,
					useCustomerName: true,
					useNickName: true,
					useMobilePhone: true,
					useAccount: true,
					useAccountType: true,
					useMoneyMoreThan: true,
					useMoneyLessThan: true,
					useStatus: true,
					useTimeType: true,
					useStartTime: true,
					useEndTime: true,
					data: {
						customerName: '',
						nickName: '',
						mobilePhone: '',
						weixinAccount: '',
						accountType: '',
						moneyMoreThan: '',
						moneyLessThan: '',
						status: '',
						timeType: '1',
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
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useCustomerName && this.search.data.customerName) {
					key.customerName = this.search.data.customerName;
					me.searchStr += ' 用户名称： ' + key.customerName;
					if(key.customerName != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useNickName && this.search.data.nickName) {
					key.nickName = this.search.data.nickName;
					me.searchStr += ' 用户昵称： ' + key.nickName;
					if(key.nickName != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useMobilePhone && this.search.data.mobilePhone) {
					key.mobilePhone = this.search.data.mobilePhone;
					me.searchStr += ' 用户手机： ' + key.mobilePhone ;
					if(key.mobilePhone != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useAccount && this.search.data.weixinAccount) {
					key.weixinAccount = this.search.data.weixinAccount;
					me.searchStr += ' 转账账号： ' + key.weixinAccount;
					if(key.weixinAccount != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useAccountType && this.search.data.accountType) {
					key.accountType = this.search.data.accountType;
					me.searchStr += ' 提现类型： ' + (key.accountType =="1"?"支付宝":"")+(key.accountType=="2"?"微信账号":"")+(key.accountType=="3"?"银行卡":"");
					if(key.accountType != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useMoneyMoreThan && this.search.data.moneyMoreThan) {
					key.moneyMoreThan = this.search.data.moneyMoreThan;
					me.searchStr += '6' + key.moneyMoreThan;
					if(key.moneyMoreThan != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useMoneyLessThan && this.search.data.moneyLessThan) {
					key.moneyLessThan = this.search.data.moneyLessThan;
					me.searchStr += '7' + key.moneyLessThan;
					if(key.moneyLessThan != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useStatus && this.search.data.status) {
					key.status = this.search.data.status;
					var temp;
					switch(parseInt(key.status)){
						case 0:
							temp ="提交申请";
							break;
						case 1:
							temp="通过申请";
							break;
						case 2:
							temp ="拒绝申请";
							break;
						case 3:
							temp ="已转账";
							break;
					}
					me.searchStr += ' 状态： ' + temp;
					if(key.status != '') {
						me.searchStr += ';';
					}
				}
//				if(this.search.useTimeType && this.search.data.timeType) {
//					key.timeType = this.search.data.timeType;
//					me.searchStr += '9' + key.timeType ;
//					if(key.timeType != '') {
//						me.searchStr += ';';
//					}
//				}
				if(this.search.useStartTime && this.search.data.startTime && this.search.useTimeType && this.search.data.timeType) {
					key.startTime = this.$filter(this.search.data.startTime, 'yyyy-mm-dd', '0');
					me.searchStr += '10' + key.startTime;
					if(key.startTime != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime && this.search.useTimeType && this.search.data.timeType) {
					key.endTime = this.$filter(this.search.data.endTime, 'yyyy-mm-dd', '0');
					me.searchStr += '11' + key.endTime;
					if(key.endTime != '') {
						me.searchStr += ';';
					}
				}
				var me = this;
				this.$get("admin/withdrawCash/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.totalMoney = response.data.totalMoney;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].accountType == 1) {
								me.table.items[i].accountType = "支付宝";
							} else if(me.table.items[i].accountType == 2) {
								me.table.items[i].accountType = "微信支付";
							} else if(me.table.items[i].accountType == 3) {
								me.table.items[i].accountType = "银行卡";
							}
							if(me.table.items[i].status == 0) {
								me.table.items[i].status = "提交申请";
							} else if(me.table.items[i].status == 1) {
								me.table.items[i].status = "不启用";
							} else if(me.table.items[i].status == 2) {
								me.table.items[i].status = "不启用";
							} else if(me.table.items[i].status == 3) {
								me.table.items[i].status = "已转账";
							}
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
<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询汇总记录" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center"><el-checkbox label="支付方式" v-model="search.usePayType" style="margin-top: 5px"></el-checkbox></el-col>
						<el-col :span="16">
							<el-select style="width: 100%;" multiple v-model="search.data.payType" placeholder="请选择支付方式" :disabled="!search.usePayType">
								<el-option label="请选择支付方式" value=""></el-option>
								<el-option label="微信支付" value="1"></el-option>
								<el-option label="支付宝支付" value="2"></el-option>
								<el-option label="银联支付" value="3"></el-option>
								<el-option label="余额支付" value="4"></el-option>
								<el-option label="团购卡支付" value="5"></el-option>
								<el-option label="积分兑换支付" value="6"></el-option>
							</el-select>
						</el-col>
					</el-row>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center"><el-checkbox label="结束时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox></el-col>
						<el-col :span="16">
							<el-date-picker
								v-model="search.data.endTime"
								:disabled="!search.useEndTime"
								style="width:100%"
								type="date"
								placeholder="选择结束时间"
								:picker-options="pickerOptions"
							></el-date-picker>
						</el-col>
					</el-row>
				</el-col>

				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center"><el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox></el-col>
						<el-col :span="16">
							<el-date-picker
								v-model="search.data.startTime"
								:disabled="!search.useStartTime"
								style="width:100%"
								type="date"
								placeholder="选择开始时间"
								:picker-options="pickerOptions"
							></el-date-picker>
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
			<el-col :span="4"><bc-statbox title="总销售额" v-bind:part1="totalPayable" back="#409EFF"></bc-statbox></el-col>
			<el-col :span="4"><bc-statbox title="总退款金额" v-bind:part1="totalRefundCash" back="#409EFF"></bc-statbox></el-col>
			<el-col :span="4"><bc-statbox title="总手续费" v-bind:part1="totalPayRateSum" back="#409EFF"></bc-statbox></el-col>
			<el-col :span="4"><bc-statbox title="总税额" v-bind:part1="totalTaxCost" back="#409EFF"></bc-statbox></el-col>
			<el-col :span="4"><bc-statbox title="总划账金额" v-bind:part1="totalBalance" back="#409EFF"></bc-statbox></el-col>
		</el-row>
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;">{{ searchStr }}</span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-tabs v-model="tabIndex" @tab-click="handleClick">
						<el-tab-pane label="销售汇总" name="0">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="payTime" label="支付日期"></el-table-column>
								<el-table-column prop="payType" label="支付方式"></el-table-column>
								<el-table-column prop="source" label="平台类型"></el-table-column>
								<el-table-column prop="tradeType" label="消费类型"></el-table-column>
								<el-table-column prop="totalPayable" label="销售总额"></el-table-column>
								<el-table-column prop="refundCash" label="退款总额"></el-table-column>
								<el-table-column prop="payRateSum" label="手续费"></el-table-column>
								<el-table-column prop="totalTaxCost" label="税额"></el-table-column>
								<el-table-column prop="balance" label="划账金额"></el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="支付方式汇总" name="1">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="startTime" label="支付日期从"></el-table-column>
								<el-table-column prop="endTime" label="至支付日期"></el-table-column>
								<el-table-column prop="payType" label="支付方式"></el-table-column>
								<el-table-column prop="tradeType" label="交易类型"></el-table-column>
								<el-table-column prop="totalPayable" label="销售总额"></el-table-column>
								<el-table-column prop="refundCash" label="退款总额"></el-table-column>
								<el-table-column prop="payRateSum" label="手续费"></el-table-column>
								<el-table-column prop="totalTaxCost" label="税额"></el-table-column>
								<el-table-column prop="balance" label="划账金额"></el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="平台类型汇总" name="2">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="startTime" label="支付日期从"></el-table-column>
								<el-table-column :formatter="formatter" prop="endTime" label="至支付日期"></el-table-column>
								<el-table-column prop="source" label="平台类型"></el-table-column>
								<el-table-column prop="totalPayable" label="销售总额"></el-table-column>
								<el-table-column prop="refundCash" label="退款总额"></el-table-column>
								<el-table-column prop="payRateSum" label="手续费"></el-table-column>
								<el-table-column prop="totalTaxCost" label="税额"></el-table-column>
								<el-table-column prop="balance" label="划账金额"></el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="时间区间统计" name="3">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column
									prop="tag"
									label="支付日期从"
									width="150"
									:filters="startFilterArr"
									:filter-method="startFilterTag"
									filter-placement="bottom-end"
								>
									<template slot-scope="scope">
										<el-tag :type="scope.row.tag === '第一季度' ? 'primary' : 'success'" close-transition>{{ scope.row.startTime }}</el-tag>
									</template>
								</el-table-column>
								<el-table-column
									prop="tag"
									label="至支付日期"
									width="150"
									:filters="endFilterArr"
									:filter-method="endFilterTag"
									filter-placement="bottom-end"
								>
									<template slot-scope="scope">
										<el-tag :type="scope.row.tag === '第一季度' ? 'primary' : 'success'" close-transition>{{ scope.row.endTime }}</el-tag>
									</template>
								</el-table-column>
								<el-table-column prop="startTime" label="支付日期从"></el-table-column>
								<el-table-column prop="endTime" label="至支付日期"></el-table-column>
								<el-table-column prop="payType" label="支付方式"></el-table-column>
								<el-table-column prop="tradeType" label="交易类型"></el-table-column>
								<el-table-column prop="totalPayable" label="销售总额"></el-table-column>
								<el-table-column prop="refundCash" label="退款总额"></el-table-column>
								<el-table-column prop="payRateSum" label="手续费"></el-table-column>
								<el-table-column prop="totalTaxCost" label="税额"></el-table-column>
								<el-table-column prop="balance" label="划账金额"></el-table-column>
							</el-table>
						</el-tab-pane>
					</el-tabs>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<!-- <el-button type="info" size="small" v-if="tabIndex == 0" @click="exportByOrde(1)"> -->
								<el-button type="info" size="small" @click="exportByOrde(1)">
									<i class="icon-arrow-down-circle icons"></i>
									导出
								</el-button>
								<!-- <el-button type="info" size="small" v-if="tabIndex==0" @click="exportByOrde(2)"><i class="icon-arrow-down-circle icons"></i> 导出产品品类汇总表</el-button> -->
								<!-- <el-button type="info" size="small" @click="printtf()"><i class="icon-printer icons"></i> 打印</el-button> -->
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
								:total="table.total"
							></el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
			</el-col>
		</el-row>

		<div id="pdf-wrap" style="display:none;">
			<el-row :gutter="20">
				<el-col :span="24" v-if="tabIndex == 0">销售汇总</el-col>
				<el-col :span="24" v-if="tabIndex == 1">支付方式汇总</el-col>
				<el-col :span="24" v-if="tabIndex == 2">平台类型汇总</el-col>
			</el-row>
			<el-row :gutter="20" style="margin-bottom: 20px;">
				<el-col :span="12" style="text-align: left; margin-top: 20px;">
					<div style="position: relative;left: 0px;display: inline-block;">供应商：</div>
					<div style="position: absolute;left: 300px;display: inline-block;">时间段：</div>
					<div style="position: absolute;left: 500px;display: inline-block;">订单状态：(未发货/在途/已收货/退货)：</div>
					<div style="position: absolute;left: 950px;display: inline-block;">单位：元</div>
				</el-col>
			</el-row>
			<el-row :gutter="20">
				<el-col :span="24">
					<table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
						<tr width="100%" style="border-bottom: none;">
							<td style="padding: 5px;font-size:12px;text-align:center;width: 40px">序号</td>
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
						<tr width="100%" v-for="(item, index) in table.items" style="border-bottom: none;">
							<td style="padding: 5px;font-size:12px;text-align:center;">{{ index + 1 }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{ item.supplierName }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{ item.receiveTime }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{ item.taxRateSum }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{ item.totalPayable }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;">{{ item.refundCash }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;"></td>
							<td style="padding: 5px;font-size:12px;text-align:center;border-right: 1px solid #9a9a9a;"></td>
						</tr>
						<tr width="100%" style="border-bottom: none;">
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">合计</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{ totalPayable }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{ totalThirdPayable }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{ totalDiffer }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{ totalPayable }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{ totalThirdPayable }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{ totalDiffer }}</td>
							<td style="padding: 5px;font-size:12px;text-align:center;width: 80px;border-right: 1px solid #9a9a9a;">&nbsp;</td>
						</tr>
						<tr width="100%" style="border-bottom: none;">
							<td colspan="12" style="padding: 5px;border-left:0px;border-bottom:0px;border-right:0px;font-size:12px;text-align:left;">&nbsp;</td>
						</tr>
						<tr width="100%" style="border: none;">
							<td colspan="6" style="padding: 5px;border: none;font-size:12px;text-align:left;">时间：{{ $filter(new Date(), '', 0) }}</td>
							<td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">制表人：</td>
							<td colspan="4" style="padding: 5px;border: none;font-size:12px;text-align:left;">审核人：</td>
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
				items: [],
				selections: [],
				olditems: []
			},
			tabIndex: 0,
			totalBalance: 0,
			totalPayRateSum: 0,
			totalPayable: 0,
			totalRefundCash: 0,
			totalTaxCost: 0,
			uploadUrl: this.baseUrl + 'admin/file/upload' + '&file=file',
			form: {
				visible: false,
				title: '',
				disabled: false,
				data: {
					startTime: '',
					endTime: '',
					file: ''
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
					endTime: ''
				}
			},
			// 筛选条件
			startFilterArr:[],
			// 筛选条件
			endFilterArr:[],
		};
	},
	methods: {
		// 初始化筛选条件
		startTimeFilter(){
			this.table.items.map(item=>{
				let str = item.startTime.substring(5, 7);
				if(this.startFilterArr.length != 0){
					this.startFilterArr.map(item1=>{
						if(str != item1.value){
							let obj = { text: str+'月', value: str }
							this.startFilterArr.push(obj)
						}
					})
				}else{
					let obj = { text: str+'月', value: str }
					this.startFilterArr.push(obj)
				}
			})
			console.log(this.startFilterArr)
		},
		// 初始化筛选条件
		endTimeFilter(){
			this.table.items.map(item=>{
				let str = item.endTime.substring(5, 7);
				if(this.endFilterArr.length != 0){
					this.endFilterArr.map(item1=>{
						if(str != item1.value){
							let obj = { text: str+'月', value: str }
							this.endFilterArr.push(obj)
						}
					})
				}else{
					let obj = { text: str+'月', value: str }
					this.endFilterArr.push(obj)
				}
			})
			console.log(this.endFilterArr)
		},
		// 重置筛选
		formatter(row, column) {
		        return row.address;
		},
		// 开始时间筛选
		startFilterTag(value, row) {
			let str = row.startTime.substring(5, 7);
			let type = false;
			if (value == '1' && str == 1) {
				type = true;
			} else if (value == '2' && str == 2) {
				type = true;
			} else if (value == '3' && str == 3) {
				type = true;
			} else if (value == '4' && str == 4) {
				type = true;
			} else if (value == '5' && str == 5) {
				type = true;
			} else if (value == '6' && str == 6) {
				type = true;
			} else if (value == '7' && str == 7) {
				type = true;
			} else if (value == '8' && str == 8) {
				type = true;
			} else if (value == '9' && str == 9) {
				type = true;
			} else if (value == '10' && str == 10) {
				type = true;
			} else if (value == '11' && str == 11) {
				type = true;
			} else if (value == '12' && str == 12) {
				type = true;
			}
			return type ? row : '';
		},
		// 最后时间筛选
		endFilterTag(value, row) {
			let str = row.startTime.substring(5, 7);
			let type = false;
			if (value == '1' && str == 1) {
				type = true;
			} else if (value == '2' && str == 2) {
				type = true;
			} else if (value == '3' && str == 3) {
				type = true;
			} else if (value == '4' && str == 4) {
				type = true;
			} else if (value == '5' && str == 5) {
				type = true;
			} else if (value == '6' && str == 6) {
				type = true;
			} else if (value == '7' && str == 7) {
				type = true;
			} else if (value == '8' && str == 8) {
				type = true;
			} else if (value == '9' && str == 9) {
				type = true;
			} else if (value == '10' && str == 10) {
				type = true;
			} else if (value == '11' && str == 11) {
				type = true;
			} else if (value == '12' && str == 12) {
				type = true;
			}
			return type ? row : '';
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
		handleDelete(id, row) {},
		handleDeleteSelections() {},
		handleClick(tab, event) {
			this.tabIndex = tab.$options.propsData.name;
			this.table.pageIndex = 1;
			this.table.pageCount = 25;
			this.handleSearch();
		},
		getSearchSection() {
			var me = this;
			var key = {};
			me.searchStr = '';
			key.length = this.table.pageCount;
			key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
			if (this.search.usePayType && this.search.data.payType.length > 0) {
				key.payType = JSON.stringify(this.search.data.payType);
				me.searchStr += ' 支付方式: ' + this.$getOrderPayTypes(this.search.data.payType);
				if (key.searchStr != '') {
					me.searchStr += '；';
				}
			}
			if (this.search.useStartTime && this.search.data.startTime) {
				key.startTime = this.$formatDate(this.search.data.startTime);
				me.searchStr += ' 开始时间：' + key.startTime;
				if (key.startTime != '') {
					me.searchStr += '；';
				}
			}
			if (this.search.useEndTime && this.search.data.endTime) {
				key.endTime = this.$formatDate(this.search.data.endTime);
				me.searchStr += ' 结束时间： ' + key.endTime;
				if (key.endTime != '') {
					me.searchStr += '；';
				}
			}
			key.status = this.tabIndex;
			return key;
		},
		handleSearch() {
			var me = this;
			me.searchStr = '搜索';
			var key = me.getSearchSection();
			this.$get('admin/order/orderSummaryList', key)
				.then2(function(response) {
					var data = response.data.data;
					for (var i = 0; i < data.length; i++) {
						data[i].payType = me.$getOrderPayType(data[i].payType);
						data[i].source = me.$getOrderSource(data[i].source);
						data[i].startTime = response.data.startTime;
						data[i].endTime = response.data.endTime;
					}
					me.table.items = data;
					me.startTimeFilter()
					me.endTimeFilter()
					me.table.total = response.data.totalRow;
					me.totalBalance = response.data.totalBalance;
					me.totalPayRateSum = response.data.totalPayRateSum;
					me.totalPayable = response.data.totalPayable;
					me.totalRefundCash = response.data.totalRefundCash;
					me.totalTaxCost = response.data.totalTaxCost;
					me.search.visible = false;
				})
				.catch(function(err) {
					console.log(err);
				});
		},
		//导出Excel表
		exportByOrde: function(type) {
			if (type == 1) {
				url = 'admin/order/exportByOrderSource';
			} else {
				url = 'admin/excel/exportByOrderTax';
			}
			var key = this.getSearchSection();

			this.$get(url, key)
				.then2(function(response) {
					if (response.data.error == 0) {
						window.open(response.data.path);
					} else {
						return false;
					}
				})
				.catch(function(err) {
					console.log(err);
				});
		},
		handleAvatarSuccess(res) {
			this.form.data.file = res.result.file;
		},
		printtf: function() {
			this.bundprint('pdf-wrap');
		}
	},
	mounted: function() {
		this.handleSearch();
	}
};
</script>

<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询活动" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入活动关键字" v-model="search.data.promotionTitle" :disabled="!search.usePromotionTitle" class="bottom20 input-size">
						<el-checkbox label="活动名称" slot="prepend" v-model="search.usePromotionTitle"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入现金优惠" v-model="search.data.cashDiscount" :disabled="!search.useCashDiscount" class="bottom20 input-size">
						<el-checkbox label="现金优惠" slot="prepend" v-model="search.useCashDiscount"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入折扣优惠" v-model="search.data.percentageDiscount" :disabled="!search.usePercentageDiscount" class="bottom20 input-size">
						<el-checkbox label="折扣优惠" slot="prepend" v-model="search.usePercentageDiscount"></el-checkbox>
					</el-input>

				</el-col>
				<el-col :span="12">

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="类型" v-model="search.useCashOrPercentageDiscount" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%; margin-left:10px" v-model="search.data.cashOrPercentageDiscount" placeholder="请选择类型" :disabled="!search.useCashOrPercentageDiscount">
								<el-option label="现金" value="1"></el-option>
								<el-option label="折扣" value="2"></el-option>
							</el-select>
						</el-col>
					</el-row>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="开始日期" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16" style="text-align: center">
							<el-date-picker style="margin-top: 5px; margin-left: 10px;" v-model="search.data.startTime" type="date" format="yyyy-MM-dd" placeholder="选择开始日期" :disabled="!search.useStartTime">
							</el-date-picker>
						</el-col>
					</el-row>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left">
							<el-checkbox label="结束日期" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16" style="text-align: center">
							<el-date-picker style="margin-top: 5px; margin-left: 10px;" v-model="search.data.endTime" type="date" format="yyyy-MM-dd" placeholder="选择结束日期" :disabled="!search.useEndTime">
							</el-date-picker>
						</el-col>
					</el-row>

				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<!--  <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="位置总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="首页推荐" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="首页广告" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="内页推荐" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row> -->
		<el-row :gutter="20">
			<!--<el-col :span="4">
				<bc-panel title="查询活动" icon="el-icon-search">
					<div slot="footer" style="text-align: right">
						<el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
					</div>
					<el-input placeholder="请输入活动关键字" v-model="search.data.promotionTitle" :disabled="!search.usePromotionTitle" class="bottom20">
						<el-checkbox label="活动名称" slot="prepend" v-model="search.usePromotionTitle"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="类型" v-model="search.useCashOrPercentageDiscount" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="17">
							<el-select v-model="search.data.cashOrPercentageDiscount" placeholder="请选择类型" :disabled="!search.useCashOrPercentageDiscount">
								<el-option label="现金" value="1"></el-option>
								<el-option label="折扣" value="2"></el-option>
							</el-select>
						</el-col>
					</el-row>
					<el-input placeholder="请输入现金优惠" v-model="search.data.cashDiscount" :disabled="!search.useCashDiscount" class="bottom20">
						<el-checkbox label="现金优惠" slot="prepend" v-model="search.useCashDiscount"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入折扣优惠" v-model="search.data.percentageDiscount" :disabled="!search.usePercentageDiscount" class="bottom20">
						<el-checkbox label="折扣优惠" slot="prepend" v-model="search.usePercentageDiscount"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="开始时间：" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-date-picker v-model="search.data.startTime" type="date" placeholder="选择日期" :disabled="!search.useStartTime">
						</el-date-picker>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="结束时间：" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-date-picker v-model="search.data.endTime" type="date" placeholder="选择日期" :disabled="!search.useEndTime">
						</el-date-picker>
					</el-row>
				</bc-panel>
			</el-col>-->
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					{{searchStr}}
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="title" label="活动名称">
						</el-table-column>
						<el-table-column prop="type" label="类型">
						</el-table-column>
						<el-table-column prop="cashDiscount" label="现金">
						</el-table-column>
						<el-table-column prop="percentageDiscount" label="折扣">
						</el-table-column>
						<el-table-column prop="startDate" label="开始时间">
						</el-table-column>
						<el-table-column prop="endDate" label="截止时间">
						</el-table-column>
						<el-table-column prop="details" label="商品">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit1(scope.$index, scope.row)">商品</el-button>
							</template>
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
								<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>
								<el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>
						<el-col :span="16" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" label-width="120px">
						<el-form-item label="活动名称">
							<el-input v-model="form.data.promotionTitle" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="适用范围">
							<el-select v-model="form.data.basedOn" placeholder="请选择推荐类型" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="订单" value="1"></el-option>
								<el-option label="所有产品购买" value="2"></el-option>
								<el-option label="所有服务购买" value="3"></el-option>
								<el-option label="特定产品购买" value="4"></el-option>
								<el-option label="特定服务购买" value="5"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="适用条件">
							<el-select v-model="form.data.conditions" placeholder="请选择推荐类型" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="无条件折扣" value="1"></el-option>
								<el-option label="达成最低额是折扣" value="2"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="最低购买金额">
							<el-input v-model="form.data.minPurchaseAmount" :disabled="form.disabled" placeholder="享有折扣的最低购买金额"></el-input>
						</el-form-item>
						<el-form-item label="类型">
							<el-select v-model="form.data.promotionType" placeholder="请选择推荐类型" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="现金" value="2"></el-option>
								<el-option label="折扣" value="1"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="开始时间">
							<el-date-picker v-model="form.data.startTime" type="date" placeholder="选择日期">
							</el-date-picker>
						</el-form-item>
						<el-form-item label="截止时间">
							<el-date-picker v-model="form.data.endTime" type="date" placeholder="选择日期">
							</el-date-picker>
						</el-form-item>
						<el-form-item label="活动说明">
							<el-input v-model="form.data.promotionDescription" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item>

							<el-button @click="form.visible=false">取消</el-button>
							<el-button type="primary" @click="handleSubmit()">保存</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				<el-dialog :title="table1.title" v-model="table1.visible" size="small">
					<el-row>
						<el-table :data="table1.items" border @selection-change="handleSelectionChange" style="width: 100%">
							<el-table-column type="selection" width="55">
							</el-table-column>
							<el-table-column prop="id" label="序号">
							</el-table-column>
							<el-table-column prop="name" label="商品名称">
							</el-table-column>
							<el-table-column label="操作" width="150">
								<template slot-scope="scope">
									<el-button size="small" @click="handleEdit1(scope.$index, scope.row)">编辑</el-button>
									<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
								</template>
							</el-table-column>
						</el-table>
					</el-row>
					<el-row slot="footer">
						<el-col :span="9">
							<el-button-group>
								<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>
								<el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>
						<el-col :span="15" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
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
							id: '',
							title: '',
							type: '',
							cashDiscount: '',
							percentageDiscount: '',
							startDate: '',
							endDate: ''
					},],
					selections: [],
				},
				cashOrPercentageDiscounts: {
					1: "现金",
					2: "折扣"
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						promotionTitle: '',
						promotionType: '',
						scope: '',
						startTime: '',
						promotionDescription: '',
						endTime: '',
						basedOn: '',
						appliedTo: '',
						cashOrPercentageDiscount: '',
						cashDiscount: '',
						conditions: '',
						id: '',
						minPurchaseAmount: '',
						promPic: ''
					}
				},
				table1: {
					visible: false,
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					title: '',
					items: [{
						cashDiscount: '',
						commissionPercentage: '',
						id: '',
						minPurchaseAmount: '',
						name: '',
						product_id: '',
						promotion_id: '',
						promotion_position_id: '',
						storeAmount: ''
					}]
				},
				search: {
					visible: false,
					usePromotionTitle: true,
					useCashOrPercentageDiscount: true,
					useCashDiscount: true,
					usePercentageDiscount: true,
					useStartTime: true,
					useEndTime: true,
					data: {
						promotionTitle: '',
						cashOrPercentageDiscount: '',
						cashDiscount: '',
						percentageDiscount: '',
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
				this.form.sortNumber = null;
				this.form.data.name = '';
				this.form.data.type = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改记录";
				me.form.visible = true;

				this.$get('admin/promotion/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.category;
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			handleEdit1(id, row) {
				var me = this;
				me.table1.title = "活动商品";
				me.table1.visible = true;
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

				if(this.search.usePromotionTitle && this.search.data.promotionTitle) {
					key.promotionTitle = this.search.data.promotionTitle;
					me.searchStr += '"' + key.promotionTitle + '"';
				}
				if(this.search.useCashOrPercentageDiscount && this.search.data.cashOrPercentageDiscount) {
					key.cashOrPercentageDiscount = this.search.data.cashOrPercentageDiscount;
				}
				if(this.search.useCashDiscount && this.search.data.cashDiscount) {
					key.cashDiscount = this.search.data.cashDiscount;
				}
				if(this.search.usePercentageDiscount && this.search.data.percentageDiscount) {
					key.percentageDiscount = this.search.data.percentageDiscount;
				}
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$filter(this.search.data.startTime, 'yyyy-mm-dd', '1');
					me.searchStr += '"' + key.startTime + '"';
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$filter(this.search.data.endTime, 'yyyy-mm-dd', '1');
				}
				var me = this;
				this.$get('admin/promotion/many', key)
					.then2(function(response) {
						for(var i = 0; i < response.data.data.length; i++) {
							me.table.items = response.data.data;
							if(response.data.data[i].type == 1) {
								me.table.items[i].type = '现金';
							} else if(response.data.data[i].type == 2) {
								me.table.items[i].type = '折扣';
							}
//							response.data.data[i].cashOrPercentageDiscount = me.cashOrPercentageDiscounts[response.data.data[i].cashOrPercentageDiscount];
						}
						me.table.total = response.data.totalRow;
						me.search.visible = false;
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
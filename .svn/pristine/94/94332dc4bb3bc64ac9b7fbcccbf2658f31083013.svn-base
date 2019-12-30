<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询等级" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入会员名称" v-model="search.data.customerName" :disabled="!search.useCustomerName" class="bottom20" style="width: 87%;">
						<el-checkbox label="会员" slot="prepend" v-model="search.useCustomerName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>
				<el-col :span="12">
					<el-input placeholder="请输入产品标题" v-model="search.data.productName" :disabled="!search.useProductName" class="bottom20" style="width: 87%;">
						<el-checkbox label="产品标题" slot="prepend" v-model="search.useProductName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="结束时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<!-- <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="人员总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row> -->
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="customer_name" label="会员">
						</el-table-column>
						<el-table-column prop="product_name" label="产品名称">
						</el-table-column>
						<el-table-column prop="supplier_name" label="供应商">
						</el-table-column>
						<el-table-column prop="create_at" label="浏览时间">
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>-->
								<!-- <el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button> -->
							</el-button-group>
						</el-col>
						<el-col :span="24" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
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
					items: [],
					items1: [],
					selections: [],
				},
				gradename: [],
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						disabled: '',
						id: '',
						nickName: '',
						password: '',
						role_id: '',
						roleId: '',
						userName: ''
					},
					data1: {
						disabled: '',
						id: '',
						nickName: '',
						password: '',
						role_id: '',
						roleId: '',
						userName: ''
					}
				},
				search: {
					visible: false,
					useCustomerName: true,
					useProductName: true,
					useStartTime: true,
					useEndTime: true,
					data: {
						customerName: '',
						productName: '',
						startTime: '',
						endTime: '',
					}
				},
				rules: {
					name: [{
						required: true,
						message: '名称不能为空',
						trigger: 'blur'
					}],
					start: [{
						required: true,
						message: '最小值不能为空'
					}],
					end: [{
						required: true,
						message: '最大值不能为空'
					}]
				}
			}
		},
		methods: {
			pickerOptions(){

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
			handleCreate() {
				this.form.title = "创建等级规则配置";
				this.form.disabled = false;
				this.form.data.id = '';
				this.form.data.name = '';
				this.form.data.start = '';
				this.form.data.end = '';
				this.form.data.vip = '';
				this.form.data.original = '';
				this.form.data.target = '';
				this.form.data.note = '';
				this.form.visible = true;
			},
			handleEdit(){
				this.$router.push('mvp_analyze_details')
			},
			
			
			messageBoxTips(title, msg) {
				this.$alert(msg, title, {
					confirmButtonText: '确定',
				});
			},
			handleSubmit() {

				var validFlag = true;
				this.$refs.form.validate(function(valid) {
					if(!valid) {
						validFlag = false;
					}
				});

				if(!validFlag) {
					this.messageBoxTips("温馨提示", "星标字段不能为空");
					return;
				}

				var me = this;
				var data = me.form.data;
				//alert(JSON.stringify(me.form.data1));
				//alert(data1.end);

				if(me.form.data1 != null) {
					if(me.form.data1.end != null) {
						if(!(data.start >= me.form.data1.end)) {
							this.messageBoxTips("温馨提示", "最小值不能小于上个等级的最大值");
							return false;
						}
					}
				}
				if(parseInt(data.start) > parseInt(data.end)) {
					this.messageBoxTips("温馨提示", "最小值不能大于最大值");
					return false;
				}

				/*if(!data.userName) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}*/
				if(data.id) {
					this.$post('admin/customer/updateGrade', data).then2(function(response) {
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
				} else {
					/*if(me.form.data.vip != '') {
						this.$get('admin/customer/getOneGrade', {
								id: me.form.data.vip - 1
							}).then2(function(response) {
								//me.form.data1 = response.data.data;
								//alert(response.data.data);
								if(response.data.data.end != null) {
									//alert(response.data.data.end);
									console.log("xiao=" + data.start);
									console.log("da=" + response.data.data.end);
									if(!(data.start >= response.data.data.end)) {
										me.messageBoxTips("温馨提示", "最小值不能小于上个等级的最大值");
										me.handleReturnFalse();
										flag = true;
									}
									//return false;
								}
							}),
							function(response) {
								me.$notify.error({
									title: '错误',
									message: '查看失败',
								});
							};
					}*/
						this.$post('admin/customer/createGrade', data).then2(function(response) {
							me.$notify({
								title: '成功',
								message: '保存成功',
								type: 'success'
							});
							me.handleSearch();
							me.form.visible = false;
						}, function(response) {
							me.$notify.error({
								title: '错误',
								message: '保存失败',
							});
							me.form.visible = false;
						});
				}
			},
			handleReturnFalse() {
				return false;
			},
			handleSearch() {
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.count = this.table.pageCount;
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useCustomerName && this.search.data.customerName) {
					key.customerName = this.search.data.customerName;
					me.searchStr += ' 会员： ' + key.customerName;
					if(key.customerName != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useProductName && this.search.data.productName) {
					key.productName = this.search.data.productName;
					me.searchStr += ' 产品名称： ' + key.productName;
					if(key.productName != '') {
						me.searchStr += ',';
					}
				}
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$formatDate(this.search.data.startTime);
					me.searchStr += ' 开始时间： ' + key.startTime;
					if(key.startTime != '') {
						me.searchStr += ',';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$formatDate(this.search.data.endTime);
					me.searchStr += ' 结束时间： ' + key.endTime;
					if(key.endTime != '') {
						me.searchStr += ',';
					}
				}

				var me = this;

				this.$get("admin/lookRecord/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});

			},
			handleBetton() {
				var me = this;

			},
		},
		mounted: function() {
			this.handleSearch();
			//this.handleBetton();
		}
	}
</script>
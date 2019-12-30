<template scoped>
	<div>
		<!--  
      <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="人员总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row>
 -->
		<!--查询弹出框-->
		<el-dialog title="查询店铺信息" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="店铺名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入联系电话号" v-model="search.data.phone" :disabled="!search.usePhone" class="bottom20 input-size">
						<el-checkbox label="联系电话" slot="prepend" v-model="search.usePhone"></el-checkbox>
					</el-input>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left;margin-left: 10px;">
							<el-checkbox label="类型" v-model="search.useShopType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%; margin-left: -10px;" v-model="search.data.shopType" placeholder="请选择商铺类型" :disabled="!search.useShopType">
								<el-option label="请选择商铺类型" value=""></el-option>
								<el-option label="第三方运营" value="1"></el-option>
								<el-option label="O2O服务区" value="2"></el-option>
								<el-option label="自营" value="3"></el-option>
							</el-select>
						</el-col>
					</el-row>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left; margin-left: 10px;">
							<el-checkbox label="状态" v-model="search.useStatus" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%; margin-left: -10px;" v-model="search.data.status" placeholder="请选择状态类型" :disabled="!search.useStatus">
								<el-option label="请选择状态类型" value=""></el-option>
								<el-option label="待审核" value="0"></el-option>
								<el-option label="通过" value="1"></el-option>
								<el-option label="不通过" value="2"></el-option>
								<el-option label="禁用" value="3"></el-option>
							</el-select>
						</el-col>
					</el-row>

				</el-col>

				<el-col :span="12">

					<el-input placeholder="请输入联系人名称关键字" v-model="search.data.contacts" :disabled="!search.useContacts" class="bottom20 input-size">
						<el-checkbox label="联系人" slot="prepend" v-model="search.useContacts"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入会员手机号" v-model="search.data.customer_phone" :disabled="!search.useCustomer_phone" class="bottom20 input-size">
						<el-checkbox label="会员手机号" slot="prepend" v-model="search.useCustomer_phone"></el-checkbox>
					</el-input>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="开始" v-model="search.useStartTime" style="margin-top: 5px; margin-left: -20px;"></el-checkbox>
						</el-col>
						<el-col :span="16" style="text-align: center">
							<el-date-picker style="margin-top: 5px; margin-left: -10px;" v-model="search.data.startTime" type="date" format="yyyy-MM-dd" placeholder="选择开始日期" :disabled="!search.useStartTime">
							</el-date-picker>
						</el-col>
					</el-row>

					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="结束" v-model="search.useEndTime" style="margin-top: 5px; margin-left: -20px;"></el-checkbox>
						</el-col>
						<el-col :span="16" style="text-align: center">
							<el-date-picker style="margin-top: 5px; margin-left: -10px;" v-model="search.data.endTime" type="date" format="yyyy-MM-dd" placeholder="选择开始日期" :disabled="!search.useEndTime">
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
		<el-row :gutter="20">
			<!--<el-col :span="4">
				<bc-panel title="查询店铺信息" icon="el-icon-search">
					<div slot="footer" style="text-align: right">
						<el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
					</div>
					<el-input placeholder="请输入会员手机号" v-model="search.data.customer_phone" :disabled="!search.useCustomer_phone" class="bottom20">
						<el-checkbox label="会员手机号" slot="prepend" v-model="search.useCustomer_phone"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
						<el-checkbox label="店铺名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="类型" v-model="search.useShopType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="17">
							<el-select v-model="search.data.shopType" placeholder="请选择商铺类型" :disabled="!search.useShopType">
								<el-option label="请选择商铺类型" value=""></el-option>
								<el-option label="第三方运营" value="1"></el-option>
								<el-option label="O2O服务区" value="2"></el-option>
								<el-option label="自营" value="3"></el-option>
							</el-select>
						</el-col>
					</el-row>
					<el-input placeholder="请输入联系人名称关键字" v-model="search.data.contacts" :disabled="!search.useContacts" class="bottom20">
						<el-checkbox label="联系人" slot="prepend" v-model="search.useContacts"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入联系电话号" v-model="search.data.phone" :disabled="!search.usePhone" class="bottom20">
						<el-checkbox label="联系电话" slot="prepend" v-model="search.usePhone"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="状态" v-model="search.useStatus" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="17">
							<el-select v-model="search.data.status" placeholder="请选择状态类型" :disabled="!search.useStatus">
								<el-option label="请选择状态类型" value=""></el-option>
								<el-option label="待审核" value="0"></el-option>
								<el-option label="通过" value="1"></el-option>
								<el-option label="不通过" value="2"></el-option>
								<el-option label="禁用" value="3"></el-option>
							</el-select>
						</el-col>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="开始" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-date-picker v-model="search.data.startTime" type="date" placeholder="选择申请日期" :disabled="!search.useStartTime">
						</el-date-picker>
					</el-row>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="结束" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-date-picker v-model="search.data.endTime" type="date" placeholder="选择申请日期" :disabled="!search.useEndTime">
						</el-date-picker>
					</el-row>
				</bc-panel>
			</el-col>-->
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-tabs v-model="tabIndex" @tab-click="handleClick">
						<el-tab-pane label="待审核店铺" name="0">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="phone" label="会员手机号">
								</el-table-column>
								<el-table-column prop="name" label="店铺名称">
								</el-table-column>
								<el-table-column prop="shopType" label="类型">
								</el-table-column>
								<el-table-column prop="contacts" label="联系人">
								</el-table-column>
								<el-table-column prop="phone" label="联系电话">
								</el-table-column>
								<el-table-column prop="created_at" label="申请时间">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit(scope.$index, scope.row)">审核</el-button>
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
						<el-tab-pane label="已审核店铺" name="1">
							<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
								<el-table-column prop="phone" label="会员手机号">
								</el-table-column>
								<el-table-column prop="name" label="店铺名称">
								</el-table-column>
								<el-table-column prop="shopType" label="类型">
								</el-table-column>
								<el-table-column prop="contacts" label="联系人">
								</el-table-column>
								<el-table-column prop="phone" label="联系电话">
								</el-table-column>
								<el-table-column prop="status" label="状态">
								</el-table-column>
								<el-table-column prop="created_at" label="创建时间">
								</el-table-column>
								<el-table-column prop="updated_at" label="审核时间">
								</el-table-column>
								<el-table-column label="操作">
									<template slot-scope="scope">
										<el-button size="small" @click="handleEdit1(scope.$index, scope.row)">查询</el-button>
										<!--           <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button> -->
									</template>
								</el-table-column>
							</el-table>
						</el-tab-pane>
					</el-tabs>
					<el-row slot="footer">
							<!--<el-button-group>
								<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 禁用
								</el-button>
								<!--              <el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
							</el-button-group>-->
						<el-col :span="24" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="ting">
					<el-form ref="form" :model="form.data" label-width="120px">
						<div>
							<hr>
						</div>
						<el-col :span="12">
							<el-form-item label="会员手机号" :label-width="formLabelWidth">
								<el-input v-model="form.data.phone" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="店铺名称" :label-width="formLabelWidth">
								<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="所在省" :label-width="formLabelWidth">
								<el-input v-model="form.data.province_id" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="所在区" :label-width="formLabelWidth">
								<el-input v-model="form.data.district_id" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="联系电话" :label-width="formLabelWidth">
								<el-input v-model="form.data.phone" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="身份证号码" :label-width="formLabelWidth">
								<el-input v-model="form.data.idcard" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="营业执照号码" :label-width="formLabelWidth">
								<el-input v-model="form.data.businessLicense" :disabled="form.disabled"></el-input>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item label="联系人">
								<el-input v-model="form.data.contacts" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="店铺类型">
								<el-input v-model="form.data.shopType" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="所在市">
								<el-input v-model="form.data.city_id" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="详细地址">
								<el-input v-model="form.data.address" :disabled="form.disabled"></el-input>
							</el-form-item>
							<el-form-item label="店铺logo">
								<img :src="form.data.logoPic" width="80%" height="80%">
							</el-form-item>
							<el-form-item label="身份证">
								<img :src="form.data.idcardPic" width="80%" height="80%">
							</el-form-item>
							<el-form-item label="营业执照">
								<img :src="form.data.businessLicensePic" width="80%" height="80%">
							</el-form-item>
						</el-col>
						<div>
							<hr>
						</div>
						<el-form-item>
							<el-button type="primary" @click="handleRefuse()">拒绝</el-button>
							<el-button @click="handlePass()">通过</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				<el-dialog :title="form1.title" v-model="form1.visible" size="ting">
					<el-form ref="form1" :model="form1.data" label-width="120px">
						<div>
							<hr>
						</div>
						<el-row>
							<el-col :span="12">
								<el-form-item label="会员手机号">
									<el-input v-model="form1.data.phone" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="店铺名称">
									<el-input v-model="form1.data.name" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="所在省">
									<el-input v-model="form1.data.province_id" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="所在区">
									<el-input v-model="form1.data.district_id" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="联系人">
									<el-input v-model="form1.data.contacts" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="身份证号码">
									<el-input v-model="form1.data.idcard" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="状态">
									<el-select v-model="form1.data.status" style="width: 100%">
										<el-option label="待审核" value="0"></el-option>
										<el-option label="通过" value="1"></el-option>
										<el-option label="不通过" value="2"></el-option>
										<el-option label="禁用" value="3"></el-option>
									</el-select>
								</el-form-item>
							</el-col>
							<el-col :span="12">
								<el-form-item label="店铺logo">
									<el-input v-model="form1.data.logoPic" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="店铺类型">
									<el-select v-model="form1.data.shopType" style="width: 100%">
										<el-option label="请选择" value="unkown"></el-option>
										<el-option label="第三方运营" value="1"></el-option>
										<el-option label="O2O服务区" value="2"></el-option>
										<el-option label="自营" value="3"></el-option>
									</el-select>
								</el-form-item>
								<el-form-item label="所在市">
									<el-input v-model="form1.data.city_id" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="详细地址">
									<el-input v-model="form1.data.address" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="联系电话">
									<el-input v-model="form1.data.phone" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<el-form-item label="营业执照">
									<el-input v-model="form1.data.businessLicensePic" :disabled="form1.disabled"></el-input>
								</el-form-item>
								<!--<el-form-item label="店铺logo">
								<img :src="form.data.logoPic" width="80%" height="80%">
							</el-form-item>-->
								<!--<el-form-item label="身份证">
								<img :src="form.data.idcardPic" width="80%" height="80%">
							</el-form-item>
							<el-form-item label="营业执照">
								<img :src="form.data.businessLicensePic" width="80%" height="80%">
							</el-form-item>-->
							</el-col>
						</el-row>
						<el-row>
							<div>
								<hr>
							</div>
							<el-form-item>
								<el-button @click="form1.visible=false">取消</el-button>
								<el-button @click="handleSubmit()">保存</el-button>
							</el-form-item>
						</el-row>
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
						name: 'haha',
						shopType: '自营',
						contacts: 'test',
						phone: '18800000000',
						created_at: '2018-02-02',
						status: '待审核',
					}, ],
					selections: [],
				},
				tabIndex: 0,
				form: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						address: '',
						businessLicense: '',
						businessLicensePic: '',
						category_id: '',
						city_id: '',
						contacts: '',
						created_at: '',
						district_id: '',
						gender: '',
						id: '',
						idcard: '',
						idcardPic: '',
						logoPic: '',
						name: '',
						note: '',
						phone: '',
						province_id: '',
						shopType: '',
						status: '',
						updated_at: ''
					}
				},
				form1: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						address: '',
						businessLicense: '',
						businessLicensePic: '',
						category_id: '',
						city_id: '',
						contacts: '',
						created_at: '',
						district_id: '',
						gender: '',
						id: '',
						idcard: '',
						idcardPic: '',
						logoPic: '',
						name: '',
						note: '',
						phone: '',
						province_id: '',
						shopType: '',
						status: '',
						updated_at: ''
					}
				},
				search: {
					visible: false,
					useCustomer_phone: true,
					useName: true,
					useShopType: true,
					useContacts: true,
					usePhone: true,
					useStartTime: true,
					useEndTime: true,
					useStatus: true,
					data: {
						customer_phone: '',
						name: '',
						shopType: '',
						contacts: '',
						phone: '',
						startTime: '',
						endTime: '',
						status: '',
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
				var me = this;
				me.form.title = "店铺审核";
				me.form.visible = true;

				this.$get('admin/shop/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(response.data.data.shopType == 1) {
							me.form.data.shopType = "第三方运营";
						} else if(response.data.data.shopType == 2) {
							me.form.data.shopType = "O2O服务区";
						} else if(response.data.data.shopType == 3) {
							me.form.data.shopType = "自营";
						}
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
				me.form1.title = "店铺信息";
				me.form1.visible = true;

				this.$get('admin/shop/get', {
						id: row.id
					}).then2(function(response) {
						me.form1.data = response.data.data;
						if(response.data.data.shopType == 1) {
							me.form1.data.shopType = "第三方运营";
						} else if(response.data.data.shopType == 2) {
							me.form1.data.shopType = "O2O服务区";
						} else if(response.data.data.shopType == 3) {
							me.form1.data.shopType = "自营";
						}
						if(response.data.data.status == 1) {
							me.form1.data.status = "通过";
						} else if(response.data.data.status == 2) {
							me.form1.data.status = "不通过";
						} else if(response.data.data.status == 3) {
							me.form1.data.status = "禁用";
						} else if(response.data.data.status == 0) {
							me.form1.data.status = "待审核";
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
				var me = this;
				var data = me.form1.data;
				if(me.form1.data.shopType == "第三方运营") {
					me.form1.data.shopType = 1;
				} else if(me.form1.data.shopType == "O2O服务区") {
					me.form1.data.shopType = 2;
				} else if(me.form1.data.shopType == "自营") {
					me.form1.data.shopType = 3;
				}
				if(me.form1.data.status == "通过") {
					me.form1.data.status = 1;
				} else if(me.form1.data.status == "不通过") {
					me.form1.data.status = 2;
				} else if(me.form1.data.status == "禁用") {
					me.form1.data.status = 3;
				} else if(me.form1.data.status == "待审核") {
					me.form1.data.status = 0;
				}
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				this.$post('admin/shop/update', data).then2(function(response) {
					me.$notify({
						title: '成功',
						message: '保存成功',
						type: 'success'
					});
					me.form1.visible = false;
					me.handleSearch();
				}, function(response) {
					me.$notify.error({
						title: '错误',
						message: '保存失败',
					});
					me.form1.visible = false;
				});
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
			handlePass() {
				var me = this;
				me.form.visible = false;
				this.$post('admin/shop/pass', {
						id: me.form.data.id
					}).then2(function(response) {
						if(response.data.error == 0) {
							me.$notify({
								title: '成功',
								message: '通过成功',
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
			handleClick(tab, event) {
				this.tabIndex = tab.$options.propsData.name;
				this.table.pageIndex = 1;
				this.table.pageCount = 25;
				this.handleSearch();
			},
			handleSearch() {
				//var data=this.search.data;
				if(this.tabIndex == 0) {
					url = 'admin/shop/many?status=0';
				} else {
					url = 'admin/shop/many?status=-1';
				}
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useCustomer_phone && this.search.data.customer_phone) {
					key.phone = this.search.data.customer_phone;
					me.searchStr += ' 会员手机号：' + key.phone ;
					if(key.phone != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					 me.searchStr += ' 店铺名称： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useShopType && this.search.data.shopType) {
					key.shopType = this.search.data.shopType;
					
					me.searchStr += ' 类型： ' + (key.shopType == "1"?"第三方运营":"")+(key.shopType == "2"?"o2o服务区":"")+(key.shopType == "3"?"自营":"");
					if(key.shopType != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useContacts && this.search.data.contacts) {
					key.contacts = this.search.data.contacts;
					me.searchStr += ' 联系人： ' + key.contacts;
					if(key.contacts != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.usePhone && this.search.data.phone) {
					key.phone = this.search.data.phone;
					me.searchStr += ' 联系电话： ' + key.phone;
					if(key.phone != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$filter(this.search.data.startTime, 'yyyy-mm-dd HH:mm:ss');
					me.searchStr += ' 开始： ' + key.startTime;
					if(key.startTime != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$filter(this.search.data.endTime, 'yyyy-mm-dd HH:mm:ss');
					me.searchStr += ' 结束： ' + key.endTime;
					if(key.endTime != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useStatus && this.search.data.status) {
					url = 'admin/shop/many?status=' + this.search.data.status;
				}

				var me = this;
				// alert(JSON.stringify(key));
				this.$get(url, key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
						for(var i = 0; i < response.data.data.length; i++) {
							if(response.data.data[i].shopType == 1) {
								me.table.items[i].shopType = "第三方运营";
							} else if(response.data.data[i].shopType == 2) {
								me.table.items[i].shopType = "O2O服务区";
							} else if(response.data.data[i].shopType == 3) {
								me.table.items[i].shopType = "自营";
							}
						}
						for(var i = 0; i < response.data.data.length; i++) {
							if(response.data.data[i].status == 1) {
								me.table.items[i].status = "通过";
							} else if(response.data.data[i].status == 2) {
								me.table.items[i].status = "不通过";
							} else if(response.data.data[i].status == 3) {
								me.table.items[i].status = "禁用";
							}else if(response.data.data[i].status == 0) {
								me.table.items[i].status = "待审核";
							}
						}

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
<style>
	.input-size {
		width: 87%
	}
</style>
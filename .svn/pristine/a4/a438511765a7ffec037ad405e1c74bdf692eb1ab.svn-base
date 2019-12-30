<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询供应商" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="供应商名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					
					<el-input placeholder="请输入联系人姓名" v-model="search.data.contactPerson" :disabled="!search.useContactPerson" class="bottom20 input-size">
						<el-checkbox label="联系人" slot="prepend" v-model="search.useContactPerson"></el-checkbox>
					</el-input>
					
				</el-col>

				<el-col :span="12">
					<el-input placeholder="请输入联系电话号码" v-model="search.data.phone1" :disabled="!search.usePhone1" class="bottom20 input-size">
						<el-checkbox label="联系电话" slot="prepend" v-model="search.usePhone1"></el-checkbox>
					</el-input>
					
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left; margin-left: 10px;">
							<el-checkbox label="类型" v-model="search.useType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%; margin-left: -20px;" v-model="search.data.type" placeholder="请选择类型" :disabled="!search.useType">
								<el-option label="个人性质" value="1"></el-option>
								<el-option label="公司性质" value="2"></el-option>
							</el-select>
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
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="供应商名称">
						</el-table-column>
						<el-table-column prop="contactPerson" label="联系人">
						</el-table-column>
						<el-table-column prop="phone1" label="联系电话">
						</el-table-column>
						<el-table-column prop="type" label="类型">
						</el-table-column>
						<el-table-column prop="created_at" label="创建时间">
						</el-table-column>
						<el-table-column prop="updated_at" label="修改时间">
						</el-table-column>
						<el-table-column label="操作" width="200">
							<template slot-scope="scope">
								<el-button size="small" @click="handelContract(scope.$index, scope.row)">合同</el-button>
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
								<el-button size="small" @click="handleReconciliation(scope.$index, scope.row)">对账</el-button>
								<!-- <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button> -->
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<!-- <el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button> -->
								<el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small" @click="printf()"><i class="icon-printer icons"></i> 打印</el-button>
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>
						<el-col :span="16" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="small">
					<el-form ref="form" :model="form.data" :rules="rules" label-width="80px" class="demo-ruleForm">
						<el-col :span="12" >
							<el-form-item label="供应商" prop="name">
								<el-input v-model="form.data.name" placeholder="请输入供应商名称" :disabled="form.disabled" @blur="handleCheckSupplierName()"></el-input>
							</el-form-item>
							<el-form-item label="公司类型" prop="type">
								<el-select v-model="form.data.type" placeholder="请选择推荐类型" style="width: 100%">
									<el-option label="请选择" value=""></el-option>
									<el-option label="公司性质" value="2"></el-option>
									<el-option label="个人性质" value="1"></el-option>
								</el-select>
							</el-form-item>
							<el-form-item label="所在省" prop="province_id">
								<el-select v-model="form.data.province_id" placeholder="请选择省" @change="changeProvince()" style="width: 100%">
									<el-option v-for="item in form.provinces" :key="item.id" :label="item.name" :value="item.id">
									</el-option>
								</el-select>
							</el-form-item>
							<el-form-item label="所在区" prop="district_id">
								<el-select v-model="form.data.district_id" placeholder="请选择区" style="width: 100%">
									<el-option v-for="item in form.districts" :key="item.id" :label="item.name" :value="item.id">
									</el-option>
								</el-select>
							</el-form-item>
							<el-form-item label="开户银行" prop="bankName">
								<el-input v-model="form.data.bankName"></el-input>
							</el-form-item>
							<el-form-item label="开户网点" prop="createBank">
								<el-input v-model="form.data.createBank"></el-input>
							</el-form-item>
							<el-form-item label="开户省" prop="createProvince">
								<el-input v-model="form.data.createProvince"></el-input>
							</el-form-item>
							<el-form-item label="固定电话">
								<el-input v-model="form.data.fixedTelephone" placeholder="请输入固定电话"></el-input>
							</el-form-item>
							<el-form-item label="第二联系电话">
								<el-input v-model="form.data.phone2"></el-input>
							</el-form-item>
							<el-form-item label="邮政编码">
								<el-input v-model="form.data.zipcode"></el-input>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item label="联系人" prop="contactPerson">
								<el-input v-model="form.data.contactPerson" placeholder="请输入推荐排序"></el-input>
							</el-form-item>
							<el-form-item label="法人" prop="legalPerson">
								<el-input v-model="form.data.legalPerson" placeholder="请输入法人"></el-input>
							</el-form-item>
							<el-form-item label="所有市" prop="city_id">
								<el-select v-model="form.data.city_id" placeholder="请选择市" @change="changeCity()" style="width: 100%">
									<el-option v-for="item in form.cities" :key="item.id" :label="item.name" :value="item.id">
									</el-option>
								</el-select>
							</el-form-item>
							<el-form-item label="所在街道" prop="street">
								<el-input v-model="form.data.street" placeholder="请输入推荐简介"></el-input>
							</el-form-item>
							<el-form-item label="账号" prop="bankAccount">
								<el-input v-model="form.data.bankAccount" @blur="handleCheckBankAccount()"></el-input>
							</el-form-item>
							<el-form-item label="开户名" prop="createName">
								<el-input v-model="form.data.createName"></el-input>
							</el-form-item>
							<el-form-item label="开户市" prop="createCity">
								<el-input v-model="form.data.createCity"></el-input>
							</el-form-item>
							<el-form-item label="第一联系电话">
								<el-input v-model="form.data.phone1"></el-input>
							</el-form-item>
							<el-form-item label="第三联系电话">
								<el-input v-model="form.data.phone3"></el-input>
							</el-form-item>
							<el-form-item label="备注">
								<el-input v-model="form.data.note"></el-input>
							</el-form-item>
						</el-col>
						<template>
							<el-table :data="form.data.shops" style="width: 100%">
								<el-table-column label="入驻商店" width="180">
								</el-table-column>
							</el-table>
						</template>
						<br>
						<el-form-item>
							<el-button @click="form.visible=false">取消</el-button>
							<el-button type="primary" @click="handleSubmit()">保存</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				<!-- 合同表单 -->
				<el-dialog :title="contractForm.title" v-model="contractForm.visible" size="tiny">
					<el-form ref="form" :model="contractForm.data" label-width="80px">
						<el-form-item label="账期">
							<el-select v-model="contractForm.data.account_period" placeholder="合同账期" style="width: 100%">
								<el-option
								v-for="item in contractForm.period_options"
								:key="item.value"
								:label="item.label"
								:value="item.value">
							</el-option>
						</el-select>
					</el-form-item>
					<el-form-item label="合同生效时间">
						<el-date-picker v-model="contractForm.data.start_at" type="date" placeholder="选择合同生效时间"></el-date-picker>
					</el-form-item>
					<el-form-item label="合同到期日期">
						<el-date-picker v-model="contractForm.data.end_at" type="date" placeholder="选择合同失效时间"></el-date-picker>
					</el-form-item>
					<el-form-item label="合同文件">
						<el-upload class="upload-demo" v-bind:action="uploadUrl" :on-preview="handlePreview" :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed" :file-list="fileList" :on-success="handleAvatarSuccess" :name="file">
							<el-button size="small" type="primary">点击上传</el-button>
						</el-upload>
					</el-form-item>
					<el-form-item>
						<el-button type="primary" @click="handleSubmitContract()">保存</el-button>
						<el-button @click="contractForm.visible=false">取消</el-button>
					</el-form-item>
				</el-form>
			</el-dialog>
		</el-col>
	</el-col>
</el-row>
<div id="pdf-wrap" style="display:none;">
	<el-row :gutter="20" style="margin-bottom: 20px">
		<el-col :span='24' style="text-align: center" >
			供应商查询
		</el-col>
	</el-row>
	<el-row :gutter="20" style="margin-bottom: 30px;">
		<el-col :span='12' style="text-align: left; margin-top: 20px;" >
			<div style="position: relative;left: 0px;display: inline-block;">供应商：{{strName}}</div>
			<div style="position: absolute;left: 300px;display: inline-block;">联系电话：{{strPhone1}}</div>
			<div style="position: absolute;left: 600px;display: inline-block;">联系人：{{strContactPerson}}</div>
			<div style="position: absolute;left: 800px;display: inline-block;width:230px;">类型：{{strType}}</div>
		</el-col>
	</el-row>
	<el-row :gutter="20" >
		<el-col :span='24'>
			<table width="100%" border="1" cellspacing="0" cellpadding="0" style="border-left:0px;border-top:0px;border-bottom:0px;border-right:0px;">
				<tr width="100%" style="border-bottom: none;">
					<td style="padding: 5px;font-size:12px;text-align:center;width: 40px">序号</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 50px">供应商</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 50px">联系人</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">公司类型</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 50px">法人</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 50px">所在省</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">所在市</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">所在区</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">所在街道</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">开户银行</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">账号</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">开户名</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px">固定电话</td>
					<td style="padding: 5px;font-size:12px;text-align:center;width: 30px;border-right:1px solid #9a9a9a;">第一联系电话</td>
				</tr>
				<tr width="100%" v-for="(item,index) in table.list" style="border-bottom: none;">
					<td style="padding: 5px;font-size:12px;text-align:center;">{{index+1}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.name}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.contactPerson}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.type}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.legalPerson}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.province}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.city}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.district}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.street}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.bankName}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.bankAccount}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.createName}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;">{{item.fixedTelephone}}</td>
					<td style="padding: 5px;font-size:12px;text-align:center;border-right:1px solid #9a9a9a;">{{item.phone1}}</td>
				</tr>
				<tr width="100%" style="border-bottom: none;">
					<td colspan="14" style="padding: 5px;border-left:0px;border-bottom:0px;border-right:0px;font-size:12px;text-align:left;">&nbsp;</td>
				</tr>
				<tr width="100%" style="border: none;">
					<td colspan="6" style="padding: 5px;border: none;font-size:12px;text-align:left;">时间：{{$filter(new Date(),"",0)}}</td>
					<td colspan="4"  style="padding: 5px;border: none;font-size:12px;text-align:left;">制表人：</td>
					<td colspan="4"  style="padding: 5px;border: none;font-size:12px;text-align:left;">审核人：</td>
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
				uploadUrl: this.baseUrl + "admin/file/upload/",
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [],
					list: [],
					selections: [],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						legalPerson: '',
						fixedTelephone: '',
						city_id: '',
						contactPerson: '',
						created_at: '',
						districts_id: '',
						id: '',
						name: '',
						note: '',
						phone1: '',
						phone1Type: '',
						phone2: '',
						phone2Type: '',
						phone3: '',
						phone3Type: '',
						province_id: '',
						bankName: '',
						createProvince: '',
						createCity: '',
						createBank: '',
						createName: '',
						bankAccount: '',
						ratings: '',
						street: '',
						type: '',
						updated_at: '',
						zipcode: '',
					},
					provinces: [{
						id: '',
						name: ''
					}],
					cities: [{
						ProvinceID: '',
						ZipCode: '',
						id: '',
						name: ''
					}],
					districts: [{
						CityID: '',
						id: '',
						name: ''
					}]
				},
				strName: '',
				strContactPerson: '',
				strPhone1: '',
				strType: '',
				search: {
					visible: false,
					useName: true,
					useContactPerson: true,
					usePhone1: true,
					useType: true,
					data: {
						name: '',
						useContactPerson: '',
						phone1: '',
						type: ''
					}
				},
				contractForm: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						id: '',
						supplier_id: '',
						account_period: '',
						start_at: '',
						end_at: '',
						contract_file: '',
					},
					period_options:[]
				},
				formLabelWidth: '120px',
				finalFormByRules: {
					nameExist: '',
					accountExist: '',
					data: {
						supplier_name: '',
						bank_account: ''
					}
				},
				rules:{
					name: [{required: true, message: "供应商不能为空", trigger: 'blur'}],
					contactPerson: [{required: true, message: '联系人不能为空', trigger: 'blur'}],
					province_id: [{required: true, message: '请选择省份'}],
					city_id: [{required: true, message: '请选择市'}],
					district_id: [{required: true, message: '请选择区'}],
					street: [{required: true, message: '街道不能为空', trigger: 'blur'}],
					type: [{required: true, message: '请公司类型', trigger: 'blur'}],
					legalPerson: [{required: true, message: '法人姓名不能为空', trigger: 'blur'}],
					bankName: [{required: true, message: '开户银行不能为空', trigger: 'blur'}],
					bankAccount: [{required: true, message: "账号不能为空", trigger: 'blur'}],
					createBank: [{required: true, message: '开户网点不能为空', trigger: 'blur'}],
					createName: [{required: true, message: '开户名不能为空', trigger: 'blur'}],
					createProvince: [{required: true, message: '开户省份不能为空', trigger: 'blur'}],
					createCity: [{required: true, message: '开户市不能为空', trigger: 'blur'}]
				}
			}
		},
		methods: {
			handleCheckSupplierName() {
				var me = this;
				if(me.form.data.name && (me.form.data.name != me.finalFormByRules.data.supplier_name)) {
					me.$post('admin/supplier/checkSupplier', {supplierName: me.form.data.name}).then2(function(response) {
						if(response.data.error > 0) {
							me.messageBoxTips("温馨提示", response.data.errmsg);
							me.finalFormByRules.nameExist = true;
						} else {
							me.finalFormByRules.nameExist = false;
						}
					});
				}
				return me.isNotExist;
			},
			handleCheckBankAccount() {
				var me = this;
				if(me.form.data.bankAccount && (me.form.data.bankAccount != me.finalFormByRules.data.bank_account)) {
					me.$post('admin/supplier/checkBankAccount', {bankAccount: me.form.data.bankAccount}).then2(function(response) {
						if(response.data.error > 0) {
							me.messageBoxTips("温馨提示", response.data.errmsg);
							me.finalFormByRules.accountExist = true;
						} else {
							me.finalFormByRules.accountExist = false;
						}
					});
				}
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
				var me = this;
				this.form.title = "添加记录";
				this.form.sortNumber = null;
				this.form.visible = true;
				this.form.disabled = false;
				this.form.data = {};
				this.getprovinces();
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改记录";
				me.form.visible = true;
				me.form.disabled = true;
				me.form.data = {};
				this.getprovinces();
				
				this.$post('admin/supplier/get', {
					id: row.id
				}).then2(function(response) {
					response.data.data.type += "";
					me.form.data = response.data.data;
					me.finalFormByRules.data.supplier_name = response.data.data.name;
					me.finalFormByRules.data.bank_account = response.data.data.bankAccount;
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
			handleReconciliation(id, row) {
				this.$router.push({
					path: 'supplierBill', 
					query:{
						supplierId: row.id
					}
				});
			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				
				var validFlag = true;
				this.$refs.form.validate(function (valid) {
					if (!valid) {
						validFlag = false;
					}
				});


				if(!validFlag) {
					this.messageBoxTips("温馨提示", "星标字段不能为空");
					return false;
				}

				me.handleCheckSupplierName();
				me.handleCheckBankAccount();
				if(me.finalFormByRules.nameExist || me.finalFormByRules.accountExist) {
					return false;
				}
				
				if(data.id) {
					this.$post('admin/supplier/update', data).then2(function(response) {
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
					this.$post('admin/supplier/create', data).then2(function(response) {
						if(response.data.error > 0) {
							me.$notify({
								title: '警告',
								message: response.data.errmsg,
								type: 'warning'
							});
						} else {
							me.$notify({
								title: '成功',
								message: '保存成功',
								type: 'success'
							});
						}
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
			getSearchKey(key) {
				var me = this;
				me.searchStr = '搜索';

				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.strName = this.search.data.name;
					me.searchStr += ' 供应商名称： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useContactPerson && this.search.data.contactPerson) {
					key.contactPerson = this.search.data.contactPerson;
					me.strContactPerson = this.search.data.contactPerson;
					me.searchStr += ' 联系人： ' + key.contactPerson ;
					if(key.contactPerson != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.usePhone1 && this.search.data.phone1) {
					key.phone1 = this.search.data.phone1;
					me.phone1 = this.search.data.phone1;
					me.searchStr += ' 联系电话： ' + key.phone1;
					if(key.phone1 != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useType && this.search.data.type) {
					key.type = this.search.data.type;
					me.strType = me.$getSupplierType(key.type);
					me.searchStr += ' 类型 ： ' + (key.type == 1?"个人性质":"公司性质");
					if(key.type != '') {
						me.searchStr += '；';
					}
				}

				return key;
			},
			handleSearch() {
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				key = me.getSearchKey(key);

				this.$get('admin/supplier/many', key).then2(function(response) {
					for(var i = 0; i < response.data.data.length; i++) {
						response.data.data[i].type = me.$getSupplierType(response.data.data[i].type);
					}
					for(var i = 0; i < response.data.list.length; i++) {
						response.data.list[i].type = me.$getSupplierType(response.data.list[i].type);
					}
					me.table.items = response.data.data;
					me.table.list = response.data.list;
					me.table.total = response.data.totalRow;
					me.search.visible = false;
				}).catch(function(err) { 
					console.log(err);
				});
			},
			getprovinces() {
				var me = this;
				this.$post('admin/supplier/provinces').then2(function(response) {
					if(response.error > 0) {
						alert(response.errmsg);
					} else {
						me.form.provinces = response.data.provinces;
					}
				}).catch(function(err) {
					console.log(err);
				});
			},
			/**
			 * 改变省份时获取新省份的市
			 * @param provinceId
			 */
			changeProvince() {
			 	var me = this;
			 	if (!me.form.data.province_id) {
			 		return;
			 	}
			 	this.$post('admin/supplier/cities', {
			 		provinceId: me.form.data.province_id
			 	}).then2(function(response) {
			 		me.form.cities = response.data.cities;
			 	}),
			 	function(response) {
			 		me.$notify.error({
			 			title: '错误',
			 			message: '查看失败',
			 		});
			 	};
			},
			/**
			 * 改变市，获取新的地区
			 * @param cityId
			 */
			changeCity() { 
			 	var me = this;
			 	if (!me.form.data.city_id) {
			 		return;
			 	}
			 	me.$post('admin/supplier/districts', {
			 		cityId: me.form.data.city_id
			 	}).then2(function(response) {
			 		me.form.districts = response.data.districts;
			 	}),
			 	function(response) {
			 		me.$notify.error({
			 			title: '错误',
			 			message: '查看失败',
			 		});
			 	};
			},
			handelContract(id, row) {
			 	var me = this;
			 	var supplier_contract_id = row.supplier_contract_id;
			 	if (supplier_contract_id == 0) {
			 		me.contractForm.data = {};
			 		me.contractForm.visible = true;
			 	} else {
			 		me.$post('admin/supplierContract/get', {id: supplier_contract_id}).then2(function(response) {
			 			response.data.data.account_period += "";
			 			me.contractForm.data = response.data.data;
			 			me.contractForm.visible = true;
			 		}),
			 		function(response) {

			 		};
			 	}
			 	me.contractForm.data.supplier_id = row.id;
			},
			handleSubmitContract() {
			 	var me = this;
			 	var data = me.contractForm.data;

			 	if(!data.account_period) {
			 		me.$notify({
			 			title: '警告',
			 			message: '请选择账期',
			 			type: 'warning'
			 		});
			 		return false;
			 	}
			 	if(!data.start_at) {
			 		me.$notify({
			 			title: '警告',
			 			message: '请选择合同生效日期！',
			 			type: 'warning'
			 		});
			 		return false;
			 	}
			 	if(!data.end_at) {
			 		me.$notify({
			 			title: '警告',
			 			message: '请选择合同失效日期！',
			 			type: 'warning'
			 		});
			 		return false;
			 	}
			 	if(!data.contract_file) {
			 		me.$notify({
			 			title: '警告',
			 			message: '请上传合同文件！',
			 			type: 'warning'
			 		});
			 		return false;
			 	}

			 	data.start_at = me.$formatDate(data.start_at);
			 	data.end_at = me.$formatDate(data.end_at);

			 	if(data.id) {
			 		this.$post('admin/supplierContract/update', data).then2(function(response) {
			 			if(response.data.error > 0) {
			 				me.$notify({
			 					title: '警告',
			 					message: response.data.errmsg,
			 					type: 'warning'
			 				});
			 			} else {
			 				me.$notify({
			 					title: '成功',
			 					message: '保存成功',
			 					type: 'success'
			 				});
			 			}
			 			me.contractForm.visible = false;
			 			me.handleSearch();
			 		}, function(response) {
			 			me.$notify.error({
			 				title: '错误',
			 				message: '保存失败',
			 			});
			 			me.contractForm.visible = false;
			 		});
			 	} else {
			 		this.$post('admin/supplierContract/create', data).then2(function(response) {
			 			if(response.data.error > 0) {
			 				me.$notify({
			 					title: '警告',
			 					message: response.data.errmsg,
			 					type: 'warning'
			 				});
			 			} else {
			 				me.$notify({
			 					title: '成功',
			 					message: '保存成功',
			 					type: 'success'
			 				});
			 			}
			 			me.handleSearch();
			 			me.contractForm.visible = false;
			 		}, function(response) {
			 			me.$notify.error({
			 				title: '错误',
			 				message: '保存失败',
			 			});
			 			me.contractForm.visible = false;
			 		});
			 	}
			},
			handleAvatarSuccess(val) {
			 	this.contractForm.data.contract_file = val.path;
			},
			messageBoxTips(title, msg) {
			 	this.$alert(msg, title, {
			 		confirmButtonText: '确定',
			 	});
			},
			allSupplierPeriod() {
			 	var me = this;
			 	me.$post('admin/supplierContract/allSupplierPeriod', {}).then2(function(response) {
			 		me.contractForm.period_options = response.data.data;	
			 	}, function(response) {

			 	});
			},
			// 打印
	    printf:function(){
	      this.bundprint('pdf-wrap');
	    },
	    // 导出
	    handleExport:function(){
	    	var me = this;
	    	var key = me.getSearchKey({});
	    	this.$get('admin/supplier/exportSupplierList', key).then2(function(response) {
					if (response.data.error == 0) {
						window.open(response.data.path);
					} else {
						return false;
					}
				}).catch(function(err) { 
					console.log(err);
				});
	    },
		},
		mounted: function() {
			this.handleSearch();
			this.getprovinces();
			this.allSupplierPeriod();
		}
	}
</script>
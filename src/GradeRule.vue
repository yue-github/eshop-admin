<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询等级" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入等级" v-model="search.data.Name" :disabled="!search.useName" class="bottom20" style="width: 87%;">
						<el-checkbox label="等级" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<!--<el-input placeholder="请输入角色" v-model="search.data.roleName" :disabled="!search.useRoleName" class="bottom20" style="width: 87%;">
						<el-checkbox label="角色" slot="prepend" v-model="search.useRoleName"></el-checkbox>
					</el-input>-->
				</el-col>

				<!--<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="是否禁用" v-model="search.useDisabled" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%" v-model="search.data.disabled" placeholder="" :disabled="!search.useDisabled">
								<el-option label="启用" value="0"></el-option>
								<el-option label="禁用" value="1"></el-option>
							</el-select>
						</el-col>
					</el-row>
				</el-col>-->
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
			<!-- <el-col :span="4">
        <bc-panel title="查询用户" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>
          <el-input placeholder="请输入用户名" v-model="search.data.userName" :disabled="!search.useUserName" class="bottom20">
            <el-checkbox label="用户名" slot="prepend" v-model="search.useUserName"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入角色" v-model="search.data.roleName" :disabled="!search.useRoleName" class="bottom20">
            <el-checkbox label="角色" slot="prepend" v-model="search.useRoleName"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="是否禁用" v-model="search.useDisabled" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.disabled" placeholder="" :disabled="!search.useDisabled">
                <el-option label="启用" value="0"></el-option>
                <el-option label="禁用" value="1"></el-option>
              </el-select>
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
						<el-table-column prop="name" label="等级名称">
						</el-table-column>
						<el-table-column prop="start" label="等级最小值">
						</el-table-column>
						<el-table-column prop="end" label="等级最大值">
						</el-table-column>
						<el-table-column prop="vip" label="vip">
						</el-table-column>
						<el-table-column prop="created_at" label="创建时间">
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
								<el-button type="danger" size="small" @click="handleDelete(scope.$index, scope.row)" :disabled="table.items[scope.$index].notDeletedable">删除</el-button>
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>-->
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>
						<el-col :span="24" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" :rules="rules" label-width="80px" class="demo-ruleForm">
						<el-form-item label="等级名称" prop="name">
							<el-input v-model="form.data.name"></el-input>
						</el-form-item>
						<el-form-item label="等级最小值" prop="start">
							<el-input v-model="form.data.start"></el-input>
						</el-form-item>
						<el-form-item label="等级最大值" prop="end">
							<el-input v-model="form.data.end"></el-input>
						</el-form-item>
						<el-form-item label="vip">
							<el-input v-model="form.data.vip"></el-input>
						</el-form-item>
						<!--<el-form-item label="分配角色">
							<el-select v-model="form.data.role_id" placeholder="请选择" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="超级管理员" value="1"></el-option>
								<el-option label="订单管理" value="2"></el-option>
								<el-option label="财务管理" value="4"></el-option>
							</el-select>
						</el-form-item>-->
						<!--<el-form-item label="是否启用">
							<el-select v-model="form.data.disabled" placeholder="请选择" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="不启用" value="1"></el-option>
								<el-option label="启用" value="0"></el-option>
							</el-select>
						</el-form-item>-->
						<el-form-item style="text-align: right">
							<el-button @click="form.visible=false">取消</el-button>
							<el-button type="primary" @click="handleSubmit()">保存</el-button>

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
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '1',
						userName: 'haha',
						roleName: '未知',
						disabled: '正常'
					}, ],
					items1: [{
						id: '1',
						userName: 'haha',
						roleName: '未知',
						disabled: '正常'
					}, ],
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
					useName: true,
					useRoleName: true,
					useDisabled: true,
					data: {
						Name: '',
						roleName: '',
						disabled: '',
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
			handleEdit(id, row) {

				var me = this;
				me.form.title = "修改等级规则配置";
				me.form.visible = true;

				this.$get('admin/customer/getOneGrade', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};

				this.$get('admin/customer/getOneGrade', {
						id: row.id - 1
					}).then2(function(response) {
						me.form.data1 = response.data.data;
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			handleDelete(id, row) {
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/customer/deleteOneGrade', {
							id: row.id
						}).then2(function(response) {
							if(response.data.error == 0) {
								me.$notify({
									title: '成功',
									message: '删除成功',
									type: 'success'
								});
								me.handleSearch();
							}
						}),
						function(response) {
							me.$notify.error({
								title: '错误',
								message: '删除失败',
							});
						};
				}).catch(() => {

				});
			},
			handleDeleteSelections() {
				var me = this;
				var ids = [];
				var selections = me.table.selections;
				for(var i = 0; i < selections.length; i++) {
					ids.push(selections[i].id);
				}
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					me.$post('', {
							ids: JSON.stringify(ids)
						}).then2(function(response) {
							if(response.data.error == 0) {
								me.$notify({
									title: '成功',
									message: '删除成功',
									type: 'success'
								});
								me.handleSearch();
							}
						}),
						function(response) {
							me.$notify.error({
								title: '错误',
								message: '删除失败'
							});
						};
				}).catch(() => {

				});
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
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.count = this.table.pageCount;
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useName && this.search.data.Name) {
					key.name = this.search.data.Name;
					me.searchStr += ' 等级： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useRoleName && this.search.data.roleName) {
					key.roleName = this.search.data.roleName;
					me.searchStr += '"' + key.roleName + '"';
					if(key.roleName != '') {
						me.searchStr += ',';
					}
				}
				if(this.search.useDisabled && this.search.data.disabled) {
					key.disabled = this.search.data.disabled;
					me.searchStr += '"' + key.disabled + '"';
					if(key.disabled != '') {
						me.searchStr += ',';
					}
				}

				var me = this;

				this.$get("admin/customer/getGrade", key)
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
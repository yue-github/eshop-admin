<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询积分规则" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="积分规则" v-model="search.data.userName" :disabled="!search.useUserName" class="bottom20" style="width: 87%;">
						<el-checkbox label="积分规则" slot="prepend" v-model="search.useUserName"></el-checkbox>
					</el-input>
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
						<el-table-column prop="original" label="初始数值">
						</el-table-column>
						<el-table-column prop="target" label="目标数值">
						</el-table-column>
						<el-table-column prop="note" label="规则说明">
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
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
						<el-form-item label="初始数值">
							<el-input v-model="form.data.original"></el-input>
						</el-form-item>
						<el-form-item label="目标数值">
							<el-input v-model="form.data.target"></el-input>
						</el-form-item>
						<el-form-item label="说明">
							<el-input v-model="form.data.note" ></el-input>
						</el-form-item>
						<el-form-item label="规则">
							<el-input v-model="form.data.code" ></el-input>
						</el-form-item>
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
					selections: [],
				},
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
					}
				},
				search: {
					visible: false,
					useUserName: true,
					useRoleName: true,
					useDisabled: true,
					data: {
						userName: '',
						roleName: '',
						disabled: '',
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
				this.form.title = "创建积分规则";
				this.form.disabled = false;
				this.form.data.id = '';
				this.form.data.original = '';
				this.form.data.target = '';
				this.form.data.note = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改积分规则";
				me.form.visible = true;

				this.$get('admin/customer/getOneRule', {
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
			},
			handleDelete(id, row) {

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
					me.$post('admin/customer/deleteRule', {
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
					}), function(response) {
						me.$notify.error({
							title: '错误',
							message: '删除失败'
						});
					};
				}).catch(() => {

				});
			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				if(data.id) {
					this.$post('admin/customer/updateRule', data).then2(function(response) {
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
					this.$post('admin/customer/createRule', data).then2(function(response) {
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
			handleSearch() {
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.count = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useUserName && this.search.data.userName) {
					key.note = this.search.data.userName;
					me.searchStr += ' 积分规则： ' + key.note + '"';
					if(key.note != '') {
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
				this.$get("admin/customer/getPointRule", key)
					.then2(function(response) {
						me.table.items = response.data.data;
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
<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询关键字" v-model="search.visible" size="small">
			<el-form :inline="true"  :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入关键词" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="关键字" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>

				</el-col>

			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<el-row :gutter="20">
			
			<el-col :span="20">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="关键字">
						</el-table-column>
						<el-table-column prop="sort_num" label="排序">
						</el-table-column>
						<el-table-column prop="created_at" label="创建时间">
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
							<!--	<el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>-->
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
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="关键字">
							<el-input v-model="form.data.name" placeholder="属性名称" :disabled="form.disabled"></el-input>
						</el-form-item>
						
						<el-form-item label="排序">
							<el-input-number v-model="form.data.sort_num" controls-position="right" style="width: 100%">

							</el-input-number>
						</el-form-item>
						<el-form-item style="text-align: right">
							<el-button @click="form.visible=false">取消</el-button>
							<el-button type="primary" @click="handleSubmit()">保存</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				
				<el-dialog :title="form1.title" v-model="form1.visible" size="tiny">
					<el-form ref="form1" :model="form1.data" label-width="80px">
						<el-form-item label="关键字">
							<el-input v-model="form1.data.name" placeholder="属性名称"></el-input>
						</el-form-item>
						<el-form-item label="排序">
							<el-input-number style="width: 100%" v-model="form1.data.sort_num" controls-position="right">

							</el-input-number>
						</el-form-item>
						<el-form-item style="text-align: right">
							<el-button @click="form1.visible=false">取消</el-button>
							<el-button type="primary" @click="handleEditSubmit()">保存</el-button>
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
					items: [],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						name: '',
						sort_num: 0
						
					}
				},
					form1: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						id: '',
						name: '',
						sort_num: 0
						
					}
				},
				search: {
					visible: false,
					useName: true,
					data: {
						name: '',
						
					}
				}
			}
		},
		methods: {
			handleSelectionChange(val) {
				this.table.selections = val;
			},
			handleSelectionChange1(val) {
				this.table1.selections = val;
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
				this.form.title = "创建关键字";
				this.form.disabled = false;
				this.form.data.name = '';
				this.form.data.sort_num = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form1.disabled = false;
				me.form1.title = "修改关键字";
				this.form1.data.name = '';
				this.form1.data.sort_num = '';
				me.form1.visible = true;

				me.$get('admin/searchword/get', {
						id: row.id
					}).then2(function(response) {
						me.form1.data = response.data.data;
						
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			handleEditSubmit() {
				var me = this;
				var data = me.form1.data;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				
				if(data.id) {
					this.$post('admin/searchword/update', data).then2(function(response) {
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
				} 
			},
			handleDelete(id, row) {
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/searchword/delete', {
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
					me.$post('admin/searchword/batchDelete', {
							ids: ids
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
				me.handleSearch();
			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				data.name = data.name;
				data.sort_num = data.sort_num;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				
				this.$post('admin/searchword/add', data).then2(function(response) {
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
				
			},
			handleSearch() {
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.pageIndex = this.table.pageIndex;
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += ' 关键字： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				
				var me = this;
				this.$post("admin/searchword/getList", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.pageIndex = response.data.pageIndex;
						me.table.total = response.data.total;
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
			}
		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
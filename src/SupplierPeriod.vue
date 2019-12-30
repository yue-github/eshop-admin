<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询账期" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="账期类型" v-model="search.useType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%;" v-model="search.data.type" placeholder="请选择账期类型" :disabled="!search.useType">
								<el-option label="所有" value=""></el-option>
								<el-option label="日结" value="day"></el-option>
								<el-option label="月结" value="month"></el-option>
							</el-select>
						</el-col>
					</el-row>
				</el-col>

				<el-col :span="12">
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click="handleSearch()">查询</el-button>
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
						<el-table-column prop="type" label="账期类型">
						</el-table-column>
						<el-table-column prop="days" label="天数">
						</el-table-column>
						<el-table-column prop="created_at" label="创建时间">
						</el-table-column>
						<el-table-column label="编辑" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
								<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>
						<el-col style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="账期类型">
							<el-select style="width: 100%;" v-model="form.data.type" placeholder="请选择账期类型">
								<el-option label="日结" value="day"></el-option>
								<el-option label="月结" value="month"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="天数">
							<el-input v-model="form.data.days"></el-input>
						</el-form-item>
						<el-form-item>
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
					items: [{type:'',days:'',created_at:''}],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						id:'',
						type:'day',
						days:'',
					}
				},
				search: {
					visible: false,
					useType: true,
					data: {
						type: '',
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
				var me = this;
				me.form.data = {};
				me.form.title = "创建";
				me.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改";
				me.form.visible = true;

				this.$get('admin/supplierPeriod/get', {id: row.id}).then2(function(response) {
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
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/supplierPeriod/delete', {id: row.id}).then2(function(response) {
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

			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				
				if(!data.type) {
					me.$alert('账期类型不能为空', '提示', {
			          confirmButtonText: '确定',
			        });
			        return false;
				}
				if(!data.days) {
					me.$alert('天数不能为空', '提示', {
			          confirmButtonText: '确定',
			        });
					return false;
				}

				if(data.id) {
					this.$post('admin/supplierPeriod/update', data).then2(function(response) {
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
					this.$post('admin/supplierPeriod/create', data).then2(function(response) {
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
				}
			},
			handleSearch() {
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;

				if(this.search.useType && this.search.data.type) {
					key.type = this.search.data.type;
					me.searchStr += ' 账期类型：' + (key.type =="day"?"日结":"月结") ;
					if(key.type != '') {
						me.searchStr += '；';
					}
				}

				this.$get("admin/supplierPeriod/many", key).then2(function(response) {
					var data = response.data.data;
					for(var i = 0; i < data.length; i++) {
						if(data[i].type == 'day') {
							data[i].type = '日结';
						} else if (data[i].type == 'month') {
							data[i].type = '月结';
						}
					}
					me.table.items = data;
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
<style>
	.input-size {
		width: 87%
	}
</style>
<template scoped>
	<div>
		<!--查询弹出框-->
	    <el-dialog title="查询品牌" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
			    <el-col :span="12">
					<el-input placeholder="请输入标题" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="标题" slot="prepend" v-model="search.useName"></el-checkbox>
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
						<el-table-column prop="name" label="名称">
						</el-table-column>
						<el-table-column prop="note" label="描述">
						</el-table-column>
						<el-table-column prop="sortNumber" label="排序">
						</el-table-column>
						<el-table-column prop="updated_at" label="修改时间">
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
						<el-form-item label="品牌名称">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="排序">
							<el-input v-model="form.data.sortNumber" placeholder="请输入推荐排序"></el-input>
						</el-form-item>
						<el-form-item label="备注">
							<el-input v-model="form.data.note" placeholder="请输入推荐简介"></el-input>
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
						name: '',
						sortNumber: '',
						updated_at: '',
					}, ],
					selections: [],
				},
				value:'',
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						id: '',
						name: '',
						note: '',
						sortNumber: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					data: {
						name: ''
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
				this.form.sortNumber = null;
				this.form.data.name = '';
				this.form.data.sortNumber='';
				this.form.data.note='';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.visible = true;

				this.$post('admin/product_brand/get', {
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
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/product_brand/delete', {
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

			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/product_brand/update', data).then2(function(response) {
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
					this.$post('admin/product_brand/create', data).then2(function(response) {
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
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += ' 标题： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				
				key.status = this.tabIndex;
				var me = this;
				this.$get("admin/product_brand/search", key)
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
<style>
	.input-size{
		width: 87%	
	}
</style>
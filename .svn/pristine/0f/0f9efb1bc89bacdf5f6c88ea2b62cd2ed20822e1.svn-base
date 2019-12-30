<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询" v-model="search.visible" size="tiny">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
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
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="title" label="标题" width="800">
						</el-table-column>
						<el-table-column label="操作">
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
				<el-dialog :title="form.title" v-model="form.visible" size="small">
					<el-form ref="form" :model="form.data" :rules="rules" label-width="80px">
						<el-form-item label="标题" prop="title">
							<el-input v-model="form.data.title"  :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="内容">
							<WangeditorUtil v-model="form.data.content"></WangeditorUtil>
						</el-form-item>
						<el-form-item label="排序" prop="sortNumber">
							<el-input v-model="form.data.sortNumber" :disabled="form.disabled"></el-input>
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
	var divNum = 0;
	import WangeditorUtil from './WangeditorUtil.vue'
	export default {
		components: {WangeditorUtil},
		data: function() {
			return {
				searchStr: '',
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '1',
						name: 'haha'
					}, ],
					selections: [],
				},
				form: {
					ids: [],
					visible: false,
					title: '',
					disabled: false,
					data: {
						ids: [],
						id: '',
						name: '',
						content: ''
					}
				},
				defaultProps: {
					children: 'children',
					label: 'displayName'
				},
				search: {
					visible: false,
					useName: true,
					data: {
						name: ''
					}
				},
				rules: {
					title: [{
						required: true,
						message: '标题不能为空',
						trigger: 'blur'
					}],
					sortNumber: [{
						required: true,
						message: '排序值不能为空'
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
			handlewangeditorSetContent(content){
				WangeditorUtil.methods.setContent(content);
			},
			handlewangeditorGetContent(){
				return WangeditorUtil.methods.getContent();
			},
			messageBoxTips(title, msg) {
				this.$alert(msg, title, {
					confirmButtonText: '确定',
				});
			},
			handleCreate() {
				var me = this;
				
				this.form.title = "创建帮助链接";
				this.form.disabled = false;
				this.form.data.title = '';
				this.form.data.id = '';
				this.form.data.sortNumber = '';
				if(this.form.data.content != ''){
					this.handlewangeditorSetContent('');
				}
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				
				me.form.title = "修改链接";
				me.form.visible = true;
				me.form.ids = [];
				this.$get('admin/helpLink/get', {
					id: row.id
				}).then2(function(response) {
					me.form.data = response.data.data;
					me.handlewangeditorSetContent(me.form.data.content);
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
					this.$post('admin/helpLink/delete', {
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
				
				var validFlag = true;
				this.$refs.form.validate(function(valid) {
					if(!valid) {
						validFlag = false;
					}
				});
				if(!validFlag) {
					return;
				}
				var data = me.form.data;
				data.content = me.handlewangeditorGetContent();
				if(data.content == ''){
					this.messageBoxTips("温馨提示", "内容字段不能为空");
					return;
				}
				if(data.id) {
					this.$post('admin/helpLink/update', data).then2(function(response) {
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
					this.$post('admin/helpLink/create', data).then2(function(response) {
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
					key.title = this.search.data.name;
					me.searchStr += ' 标题： ' + key.title;
					if(key.title != '') {
						me.searchStr += '；';
					}
				}
				var me = this;
				this.$get("admin/helpLink/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
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
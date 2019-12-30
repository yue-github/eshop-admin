<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询税率" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入税率标签" v-model="search.data.name" :disabled="!search.useName" class="bottom20" style="width:87%">
						<el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
				</el-col>
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="是否可用" v-model="search.useDisable" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%" v-model="search.data.enable" placeholder="请选择是否启用" :disabled="!search.useDisable">
								<el-option label="请选择是否启用" value=""></el-option>
								<el-option label="启用" value="1"></el-option>
								<el-option label="不启用" value="0"></el-option>
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
					<el-row style="padding-bottom:10px;display: inline-block;"> {{searchStr}} </el-row>
					<el-row style="float:right;font-size:14px;">提示：编辑功能会统一修改和该类别税率相关产品的税率值，请谨慎操作</el-row>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="税率标签">
						</el-table-column>
						<el-table-column prop="rate" label="税率(%)">
						</el-table-column>
						<el-table-column prop="enable" label="是否可用">
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
								<!--<el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
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
						<el-form-item label="税率标签">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="税率(%)">
							<el-input-number v-model="form.data.rate" :disabled="form.disabled" controls-position="right" style="width: 100%;">
							</el-input-number>
						</el-form-item>
						<el-form-item label="是否启用">
							<el-select v-model="form.data.enable" style="width: 100%">
								<el-option label="请选择" value=""></el-option>
								<el-option label="启用" value="1"></el-option>
								<el-option label="不启用" value="0"></el-option>
							</el-select>
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
							rate: '',
							enable: '不启用'
						},
						{
							id: '2',
							name: '',
							rate: '',
							enable: '启用'
						},
					],
					selections: [],
				},
				enableMap: {
					0: '不启用',
					1: '启用'
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						name: '',
						id: 'query',
						enable: '',
						rate: ''
					}
				},
				search: {
					visible: false, //
					useName: true,
					useSex: true,
					useDisable: true,
					usePhone: true,
					data: {
						name: '',
						sex: 'unkown',
						enable: '',
						mobilePhone: ''
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
				this.form.title = "税率信息";
				this.form.disabled = false;
				this.form.data.enable = '';
				this.form.data.id = '';
				this.form.data.name = '';
				this.form.data.rate = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "税率信息";
				me.form.visible = true;

				this.$get('admin/tax/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(me.form.data.enable == 1) {
							me.form.data.enable = "启用";
						} else if(me.form.data.enable == 2) {
							me.form.data.enable = "不启用";
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
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/tax/batchDelete', {
							ids: "[" + row.id + "]"
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
					me.$post('admin/tax/batchDelete', {
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
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				if(me.form.data.enable == "启用") {
					me.form.data.enable = 1;
				} else if(me.form.data.enable == "不启用") {
					me.form.data.enable = 0;
				}
				
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请填写税率标签！',
						type: 'warning'
					});
					return false;
				}
				if(!data.enable) {
					me.$notify({
						title: '警告',
						message: '请选择是否启用状态！',
						type: 'warning'
					});
					return false;
				}
				
				if(data.id) {
					this.$confirm('此操作会统一修改和该类别税率相关产品的税率值, 是否继续?', '提示', {
			          confirmButtonText: '确定',
			          cancelButtonText: '取消',
			          type: 'warning'
			        }).then(() => {
				        this.$post('admin/tax/update', data).then2(function(response) {
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
			        }).catch(() => {
			                
			        });
					
				} else {
					this.$post('admin/tax/create', data).then2(function(response) {
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
					me.searchStr += ' 名称： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}

				if(this.search.useDisable && this.search.data.enable) {
					key.enable = this.search.data.enable;
					me.searchStr += ' 是否可用： ' + (key.enable=="1"?"启用":"不启用");
					if(key.enable != '') {
						me.searchStr += '；';
					}
				}

				var me = this;
				this.$get("admin/tax/many", key)
					.then2(function(response) {
						for(var i = 0; i < response.data.data.length; i++) {
							response.data.data[i].enable = me.enableMap[response.data.data[i].enable];
						}
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].enable == 1) {
								me.table.items[i].enable = "启用";
							} else if(me.table.items[i].enable == 0) {
								me.table.items[i].enable = "不启用";
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
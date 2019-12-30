<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询产品属性" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入分类名关键词" v-model="search.data.categoryName" :disabled="!search.useCategoryName" class="bottom20 input-size">
						<el-checkbox label="分类名" slot="prepend" v-model="search.useCategoryName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入排序值" v-model="search.data.sortNumber" :disabled="!search.useSortNumber" class="bottom20 input-size">
						<el-checkbox label="排序值" slot="prepend" v-model="search.useSortNumber"></el-checkbox>
					</el-input>

				</el-col>

				<el-col :span="12">

					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="属性名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入销售属性" v-model="search.data.is_sale_pro" :disabled="!search.useIs_sale_pro" class="bottom20 input-size">
						<el-checkbox label="销售属性" slot="prepend" v-model="search.useIs_sale_pro"></el-checkbox>
					</el-input>

				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<el-row :gutter="20">
			<el-col :span="4">
				<bc-panel title="产品分类" icon="icon-grid icons">
					<el-tree :props="props1" @node-click="handleNodeClick" :load="loadNode1" lazy>
					</el-tree>
				</bc-panel>
			</el-col>
			<el-col :span="16">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="属性名称">
						</el-table-column>
						<el-table-column prop="categoryName" label="分类名">
						</el-table-column>
						<el-table-column prop="sortNumber" label="排序值">
						</el-table-column>
						<el-table-column prop="is_sale_pro" label="销售属性">
						</el-table-column>
						<el-table-column label="属性值">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEditValue(scope.$index, scope.row)">属性值</el-button>
							</template>
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
								<el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
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
						<el-form-item label="属性名称">
							<el-input v-model="form.data.name" placeholder="属性名称" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="产品分类">
							<el-select v-model="form.data.category_id" placeholder="请选择" style="width: 100%">
								<el-option v-for="item in options" :key="item.id" :label="item.name" :value="item.id">
								</el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="是否销售属性">
							<el-select v-model="form.data.is_sale_pro" style="width: 100%">
								<el-option label="请选择" value=""></el-option>
								<el-option label="是" value="1"></el-option>
								<el-option label="否" value="0"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="排序">
							<el-input-number v-model="form.data.sortNumber" controls-position="right" style="width: 100%">

							</el-input-number>
						</el-form-item>
						<el-form-item style="text-align: right">
							<el-button @click="form.visible=false">取消</el-button>
							<el-button type="primary" @click="handleSubmit()">保存</el-button>
						</el-form-item>
					</el-form>
				</el-dialog>
				<el-dialog :title="table1.title" v-model="table1.visible" size="small">
					<el-table :data="table1.items" border @selection-change="handleSelectionChange1" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="属性值名称">
						</el-table-column>
						<el-table-column prop="sortNumber" label="排序值">
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEditValueEdit(scope.$index, scope.row)">编辑</el-button>
							</template>
						</el-table-column>
					</el-table>
					<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table1.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
					</el-pagination>
					<br/>
					<el-col>
						<el-button-group>
							<el-button type="success" size="small" @click="handleEditCreate()"><i class="icon-plus icons"></i>添加</el-button>
							<el-button type="danger" size="small" :disabled="table1.selections.length == 0" @click="handleEditDelete()"><i class="icon-minus icons"></i>删除</el-button>
							<el-button size="small" @click="table1.visible=false">取消</el-button>
						</el-button-group>
					</el-col>
					<br/>
				</el-dialog>
				<el-dialog :title="form1.title" v-model="form1.visible" size="tiny">
					<el-form ref="form1" :model="form1.data" label-width="80px">
						<el-form-item label="属性值名称">
							<el-input v-model="form1.data.name" placeholder="属性名称"></el-input>
						</el-form-item>
						<el-form-item label="排序">
							<el-input-number style="width: 100%" v-model="form1.data.sortNumber" controls-position="right">

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
					items: [{
						name: '',
						categoryName: '',
						sortNumber: '',
						is_sale_pro: '',
					}, ],
					selections: [],
				},
				props1: {
					label: 'name',
					children: 'zones',
					isLeaf: 'leaf'
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						category_id: '',
						created_at: '',
						id: '',
						isDelete: '',
						is_sale_pro: '',
						name: '',
						parent_id: '',
						sortNumber: '',
						updated_at: ''
					}
				},
				table1: {
					visible: false,
					title: '属性值',
					disabled: false,
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '',
						name: '',
						propertyName: '',
						property_id: '',
						sortNumber: '',
					}, ],
					selections: [],
				},
				form1: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						category_id: '',
						id: '',
						is_sale_pro: '',
						name: '',
						property_id: '',
						propertyId: '',
						sortNumber: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					useCategoryName: true,
					useSortNumber: true,
					useIs_sale_pro: true,
					data: {
						name: '',
						categoryName: '',
						sortNumber: '',
						is_sale_pro: '',
						category_id: ''
					}
				},
				options: [{
					id: '',
					name: '名称',
				}]
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

			handleNodeClick(data, node, vuecomponen) {
				var me = this;
				/*this.$get('admin/property/getChildCategories',{
					parent: node.data.id
				})
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							//me.table.items = response.data.data;
						}
					}).catch(function(err) {
						console.log(err)
					});*/
				//var key = {};
				//key.length = this.table.pageCount;
				//key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				this.search.data.categoryName = "";
				this.search.data.category_id = node.data.id;
				this.handleSearch();
				/*this.$post('admin/property/many', key).then2(function(response) {
					me.table.items = response.data.data;
					me.table.total = response.data.totalRow;
				}).catch(function(err) { 
					console.log(err);
				});*/
			},
			loadNode1(node, resolve) {
				var me = this;
				console.log(node);

				if(node.level === 0) {
					return resolve([{
						name: '所有',
						id: 0
					}]);
				}

				if(node.level === 1) {
					this.$get('admin/property/getChildCategories', {
							parent: 0
						}).then2(function(response) {
							//me.table.items = response.data.data;
							return resolve(response.data.data);
						}),
						function(response) {
							me.$notify.error({
								title: '错误',
								message: '查看失败',
							});
						};
					return;
				}

				if(node.level > 1) {
					parent = node.data.id;
					this.$get('admin/property/getChildCategories', {
							parent: node.data.id
						}).then2(function(response) {
							//me.table.items = response.data.data;
							return resolve(response.data.data);
						}),
						function(response) {
							me.$notify.error({
								title: '错误',
								message: '查看失败',
							});
						};
					return;
				}
			},

			handleCreate() {
				this.form.title = "创建用户";
				this.form.disabled = false;
				this.form.data.name = '';
				this.form.data.is_sale_pro = '';
				this.form.data.sortNumber = '';
				this.form.data.categoryId = '';
				this.form.data.category_id = '';
				this.form.data.id = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改记录";
				me.form.visible = true;

				this.$get('admin/property/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						//me.form.data.category_id = row.categoryName;
						if(me.form.data.is_sale_pro == 1) {
							me.form.data.is_sale_pro = '是';
						} else if(me.form.data.is_sale_pro == 0) {
							me.form.data.is_sale_pro = '否';
						}

					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			handleEditValue(id, row) {
				//alert(id + "   " +row);
				var key = {};
				var me = this;
				key.length = this.table1.pageCount;
				key.offset = (this.table1.pageIndex - 1) * this.table1.pageCount;
				key.propertyId = row.id;
				this.$get("admin/propertyValue/many", key)
					.then2(function(response) {
						me.table1.items = response.data.data;
						//me.table1.total = response.data.totalRow;
					}).catch(function(err) { 
						console.log(err);
					});
				/*alert(id + "  "+ row.id);*/
				this.table1.title = "属性值";
				this.table1.disabled = false;
				this.table1.visible = true;
			},
			newhandleEditValue(property_id) {
				var key = {};
				var me = this;
				key.length = this.table1.pageCount;
				key.offset = (this.table1.pageIndex - 1) * this.table1.pageCount;
				key.propertyId = property_id;
				this.$get("admin/propertyValue/many", key)
					.then2(function(response) {
						me.table1.items = response.data.data;
						//me.table1.total = response.data.totalRow;
					}).catch(function(err) { 
						console.log(err);
					});
				/*alert(id + "  "+ row.id);*/
				this.table1.title = "属性值";
				this.table1.disabled = false;
				this.table1.visible = true;
			},
			handleEditValueEdit(id, row) {
				var me = this;
				me.form1.title = "修改记录";
				me.form1.visible = true;
				this.$get('admin/propertyValue/get', {
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
			handleEditCreate() {
				this.form1.title = "添加记录";
				this.form1.disabled = false;
				this.form1.data.name = '';
				this.form1.data.sortNumber = '';
				this.form1.data.id = '';
				this.form1.visible = true;
			},
			handleEditDelete() {
				var me = this;
				var ids = [];
				var selections = me.table1.selections;
				//alert(selections);
				for(var i = 0; i < selections.length; i++) {
					ids.push(selections[i].id);
				}
				//var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					me.$post('admin/propertyValue/batchDelete', {
							ids: JSON.stringify(ids)
						}).then2(function(response) {
							if(response.data.error == 0) {
								me.$notify({
									title: '成功',
									message: '删除成功',
									type: 'success'
								});
								me.newhandleEditValue(me.table1.items[0].property_id);
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
				me.newhandleEditValue(me.table1.items[0].property_id);
			},
			handleEditSubmit() {
				var me = this;
				var data = me.form1.data;
				data.propertyId = data.property_id;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/propertyValue/update', data).then2(function(response) {
						me.$notify({
							title: '成功',
							message: '保存成功',
							type: 'success'
						});
						me.form1.visible = false;
						me.newhandleEditValue(me.table1.items[0].property_id);
					}, function(response) {
						me.$notify.error({
							title: '错误',
							message: '保存失败',
						});
						me.form1.visible = false;
					});
				} else {
					data.propertyId = me.table1.items[0].property_id;
					this.$post('admin/propertyValue/create', data).then2(function(response) {
						me.$notify({
							title: '成功',
							message: '保存成功',
							type: 'success'
						});
						me.newhandleEditValue(me.table1.items[0].property_id);
						me.form1.visible = false;
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
					this.$post('admin/property/batchDelete', {
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
					me.$post('admin/property/batchDelete', {
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
				me.handleSearch();
			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				if(me.form.data.is_sale_pro == '是') {
					me.form.data.is_sale_pro = 1;
				} else if(me.form.data.is_sale_pro == '否') {
					me.form.data.is_sale_pro = 0;
				}
				data.categoryId = data.category_id;
				data.isSalePro = data.is_sale_pro;
				data.parentId = data.parent_id;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/property/update', data).then2(function(response) {
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
					data.categoryId = data.category_id;
					data.parentId = data.category_id;
					this.$post('admin/property/create', data).then2(function(response) {
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
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += '分类名：' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
					
				}
				if(this.search.useCategoryName && this.search.data.categoryName) {
					key.categoryName = this.search.data.categoryName;
					me.searchStr += ' 属性名称  ：' + key.categoryName;
					if(key.categoryName != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useSortNumber && this.search.data.sortNumber) {
					key.sortNumber = this.search.data.sortNumber;
					me.searchStr += '  排序值  ：' + key.sortNumber;
					if(key.sortNumber != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useIs_sale_pro && this.search.data.is_sale_pro) {
					if(this.search.data.is_sale_pro == '是') {
						key.is_sale_pro = 1;
					} else if(this.search.data.is_sale_pro == '否') {
						key.is_sale_pro = 0;
					}
					me.searchStr += '  销售属性 ：' + this.search.data.is_sale_pro;
				}
				key.category_id = this.search.data.category_id;
				key.status = this.tabIndex;
				var me = this;
				this.$get("admin/property/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].is_sale_pro == 1) {
								me.table.items[i].is_sale_pro = '是';
							} else if(me.table.items[i].is_sale_pro == 0) {
								me.table.items[i].is_sale_pro = '否';
							}
						}
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
			},
			getAllCategories() {
				var me = this;
				this.$get('admin/property/getAllCategories')
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							me.options = response.data.data;
							//var option = [];
							/*for(var i = 0; i < data.length; i++) {
								option.push({
									value: data[i].id,
									label: data[i].name,
									children: []
								});
							}*/
							//me.options = option;
						}
					}).catch(function(err) {
						console.log(err)
					});

			}
		},
		mounted: function() {
			this.handleSearch();
			this.getAllCategories();
		}
	}
</script>
<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询产品分类" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入名称" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
				</el-col>

				<el-col :span="12">
					<el-input placeholder="请输入排序值" v-model="search.data.sortNumber" :disabled="!search.useSortNumber" class="bottom20 input-size">
						<el-checkbox label="排序值" slot="prepend" v-model="search.useSortNumber"></el-checkbox>
					</el-input>
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click="handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<el-row :gutter="20">
			<el-col :span="4">
				<bc-panel title="产品分类" icon="icon-grid icons">
					<el-tree :props="props1" @node-click="handleNodeClick" :load="loadNode1" lazy>
					</el-tree>
				</bc-panel>
			</el-col>
			<el-col :span="20">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="名称" width="150">
						</el-table-column>
						<el-table-column prop="parentName" label="上一级" width="150">
						</el-table-column>
						<el-table-column prop="sortNumber" label="排序值" width="150">
						</el-table-column>
						<el-table-column type="mainPic" label="分类图标">
							<template slot-scope="props">
								<img v-bind:src="props.row.mainPic" width="50px" height="50px" style="padding-top:5px;padding-bottom:5px;">
							</template>
						</el-table-column>
						<!-- <el-table-column prop="sale_is" label="是否预售">
							
						</el-table-column> -->
						<el-table-column prop="note" label="备注">
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
					<el-form ref="form" :model="form.data" :rules="rules" label-width="80px" class="demo-ruleForm">
						<el-form-item label="名称" prop="name">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="排序" prop="sortNumber">
							<el-input type="number" v-model="form.data.sortNumber" controls-position="right" style="width: 100%">
							</el-input>
						</el-form-item>
						<el-form-item label="上一级" prop="parent_id">
							<el-select v-model="form.data.parent_id" clearable placeholder="请选择" style="width: 100%">
								<el-option v-for="item in optionsCategories" :key="item.id" :label="item.name" :value="item.id">
								</el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="分类图标">
							<el-upload ref="upload" class="upload-demo" v-bind:action="uploadUrl" :on-preview="handlePreview" :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="1" :on-exceed="handleExceed" :file-list="fileList" :on-success="handleAvatarSuccess" :name="file">
								<el-button size="small" type="primary">点击上传</el-button>
								<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
							</el-upload>
							<img :src="show_image">
						</el-form-item>
						<!-- <el-form-item label="是否预售">
							<el-input v-model="form.data.note" placeholder="是">
							</el-input>
						</el-form-item> -->
						<el-form-item label="备注">
							<el-input v-model="form.data.note" placeholder="备注">
							</el-input>
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
				uploadUrl: this.baseUrl + "admin/file/upload",
				show_image: '',
				searchStr: '',
				props1: {
					label: 'name',
					children: 'zones',
					isLeaf: 'leaf'
				},
				treeData: {
					created_at: '',
					id: '',
					isDelete: '',
					mainPic: '',
					name: '',
					note: '',
					parent_id: '',
					sortNumber: '',
					updated_at: ''
				},
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
							created_at: '',
							id: '',
							isDelete: '',
							mainPic: '',
							name: '',
							note: '',
							parent_id: '',
							sortNumber: '',
							updated_at: '',
							parentName: '',
							sale_is:0
						},
					],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						id: '',
						created_at: '',
						name: '',
						updated_at: '',
						isDelete: '',
						mainPic: '',
						note: '',
						parent_id: '',
						parentName: '',
						sortNumber: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					useSortNumber: true,
					useNote: true,
					data: {
						name: '',
						sortNumber: '',
						note: '',
						parentId: '',
					}
				},
				options: [{
					value: 'name',
					label: '名称',
					children: [{
						value: 'parentName',
						label: '上一级'
					}]
				}],
				optionsCategories: [{
					id: '',
					name: '',
					note: '',
					parent_id: '',
					sortNumber: '',
					created_at: '',
					updated_at: '',
					isDelete: '',
					mainPic: '',
					parentName: '',
				}],
				rules:{
					name: [{required: true, message: "名称不能为空", trigger: 'blur'}],
		       		sortNumber: [
			       		{required: true, message: '排序不能为空', trigger: 'blur'},
		       		],
		       		parent_id: [{required: true, message: '请选择上一级'}],
		       	},
			}
		},

		methods: {
			handleAvatarSuccess(val) {
				this.form.data.mainPic = val.path;
				this.show_image = this.$getAbsolutePath(val.path);
				console.log('上传成功');
				console.log(val);
			},
			beforeAvatarUpload(file) {
				const isJPG = file.type === 'image/jpeg';
				const isPNG = file.type === 'image/png';
				if (!isJPG && !isPNG) {
		          this.$message.error('上传图片只能是 JPG、PNG 格式!');
		        }
		        return isJPG || isPNG;
			},
			handleRemove(file, fileList) {
				console.log('删除');
				console.log(file, fileList);
			},
			handlePreview(file) {
				console.log('预览');
				console.log(file);
			},
			handleExceed(files, fileList) {
				console.log('超出文件个数');
				this.$message.warning(`当前限制选择 1 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
			},
			beforeRemove(file, fileList) {
				console.log('删除之前');
				return this.$confirm(`确定移除 ${ file.name }？`);
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
			handleNodeClick(data, node, vuecomponen) {
				this.search.parentId = node.data.id;
				this.handleSearch();
			},
			loadNode1(node, resolve) {
				var me = this;

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
							me.table.items = response.data.data;
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
							me.table.items = response.data.data;
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
				this.form.data = {};
				this.form.visible = true;
				this.show_image = '';
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改记录";
				me.form.visible = true;

				this.$get('admin/category/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.category;
						me.form.data.sortNumber += '';
						me.form.data.parentName = row.parentName;
						me.show_image = me.$getAbsolutePath(me.form.data.mainPic);
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
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

                if (!validFlag) {
                	return false;
                }

				if(data.id) {
					this.$post('admin/category/update', data).then2(function(response) {
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
					this.$post('admin/category/create', data).then2(function(response) {
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
			handleDelete(id, row) {
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/category/batchDelete', {
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
					me.$post('admin/category/batchDelete', {
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
			getAllCategories() {
				var me = this;
				this.$get('admin/property/getAllCategories')
					.then2(function(response) {
						var first = [{
							id: 0,
							name: '顶级',
							note: '',
							parent_id: 0,
							sortNumber: 0,
							created_at: '',
							updated_at: '',
							isDelete: 0,
							mainPic: '',
							parentName: '',
						}]
						me.optionsCategories = first.concat(response.data.data);
					}).catch(function(err) {
						console.log(err)
					});

			},
			handleSearch() {
				var key = {};
				var me = this;
				me.searchStr = "搜索";
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += '  名称： ' + key.name ;
					
				}
				if(this.search.useSortNumber && this.search.data.sortNumber) {
					key.sortNumber = this.search.data.sortNumber;
					me.searchStr += '； 排序值： ' + key.sortNumber;
				}
				if(this.search.useNote && this.search.data.note) {
					key.note = this.search.data.note;
				}
				if(this.search.parentId || this.search.parentId == 0) {
					key.parentId = this.search.parentId;
				}

				this.$get("admin/category/many", key)
					.then2(function(response) {
						var list = response.data.data;
						console.log(list)
						for (var i = 0; i < list.length; i++) {
							var item = list[i];
							item.mainPic = me.$getAbsolutePath(item.mainPic);
						}
						if(!list.hasOwnProperty('sale_is')){
							me.table.items = list.map((item,index)=>{
								let random = index%9;
								random?item.sale_is='否':item.sale_is='是';
								return item;
							});
						}else{
							me.table.items = list.map(item=>{
								item.sale_is==0?item.sale_is='否':item.sale_is='是';
								return item;
							});
						}
						me.table.total = response.data.totalRow;
						me.search.visible = false;
					}).catch(function(err) {
						console.log(err);
					});
			},
			
		},
		mounted: function() {
			this.handleSearch();
			this.getAllCategories();
		}
	}
</script>
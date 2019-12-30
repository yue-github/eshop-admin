<template scoped>
	<div>
			<!--查询弹出框-->
	    <el-dialog title="查询广告" v-model="search.visible" size="tiny">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
			
			    <el-input placeholder="请输入排序值" v-model="search.data.sortNumber" :disabled="!search.useSortNumber" class="bottom20">
					<el-checkbox label="排序" slot="prepend" v-model="search.useSortNumber"></el-checkbox>
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
						<el-table-column type="expand" label="图片">
							<template slot-scope="props">
								<el-form label-position="left" inline class="demo-table-expand">
									<el-form-item label="产品图片:">
										<img v-if="props.row.path" v-bind:src="props.row.path" class="image">
									</el-form-item>
								</el-form>
							</template>
						</el-table-column>
						<el-table-column prop="url" label="超链接">
						</el-table-column>
						<el-table-column prop="sort_number" label="排序">
						</el-table-column>
						<el-table-column prop="note" label="描述">
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
						<el-form-item label="图片">
							<el-upload ref="upload" class="upload-demo" v-bind:action="uploadUrl" :on-preview="handlePreview" :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed" :file-list="fileList" :on-success="handleAvatarSuccess" :name="file">
								<el-button size="small" type="primary">点击上传</el-button>
								<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
							</el-upload>
							<img :src="show_image" width="80%" height="80%">
						</el-form-item>
						<el-form-item label="超链接">
							<el-input v-model="form.data.url"></el-input>
						</el-form-item>
						<el-form-item label="排序">
							<el-input v-model="form.data.sort_number" placeholder="请输入排序"></el-input>
						</el-form-item>
						<el-form-item label="描述">
							<el-input v-model="form.data.note" placeholder="描述"></el-input>
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
				uploadUrl: this.baseUrl + "admin/file/upload",
				show_image: '',
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '1',
						path: 'haha',
						url: '/asd/asdsad',
						sort_number: '1',
						note: '0'
					}, ],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						created_at: '',
						id: '',
						note: '',
						path: '',
						sort_number: '',
						updated_at: '',
						url: ''
					}
				},
				search: {
					visible: false,
					useSortNumber: true,
					data: {
						sortNumber: ''
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
			},
			handleCreate() {
				this.form.title = "添加记录";
				this.form.data.note = '';
				this.form.data.sort_number = '';
				this.form.data.url = '';
				this.form.data.path = '';
				this.show_image = '';
				//this.$refs.upload.clearFiles();
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				this.form.title = "修改记录";
				me.form.visible = true;

				this.$post('admin/advertisement/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						me.show_image = me.$getAbsolutePath(me.form.data.path);
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
					this.$post('admin/advertisement/batchDelete', {
							ids: '["' + row.id + '"]'
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
				if(!data.path) {
					me.$notify({
						title: '警告',
						message: '请输入path！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/advertisement/update', data).then2(function(response) {
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
					this.$post('admin/advertisement/create', data).then2(function(response) {
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
				me.searchStr = '搜索 ';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useSortNumber && this.search.data.sortNumber) {
					key.sort_number = this.search.data.sortNumber;
					me.searchStr += ' 排序： ' + key.sort_number;
					if(key.sort_number != '') {
						me.searchStr += '；';
					}
				}
				var me = this;
				this.$get('admin/advertisement/many', key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
			},
			handleRemove(file, fileList) {
				console.log(file, fileList);
			},
			handlePreview(file) {
				console.log(file);
			},
			handleExceed(files, fileList) {
				this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
			},
			beforeRemove(file, fileList) {
				return this.$confirm(`确定移除 ${ file.name }？`);
			},
			handleAvatarSuccess(val) {
				this.form.data.path = val.path;
				// this.show_image =  this.show_image + val.path;
				this.show_image =  this.$getAbsolutePath(val.path);
			}
		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
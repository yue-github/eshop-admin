<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="高级查询" v-model="search.visible" size="tiny">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">

					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
						<el-checkbox label="角色名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>

				</el-col>
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
			<!--<el-col :span="4">
        <bc-panel title="查询角色" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>
          <el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
            <el-checkbox label="角色名称" slot="prepend" v-model="search.useName"></el-checkbox>
          </el-input>
        </bc-panel>
      </el-col>-->
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="角色名称" width="800">
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
								<!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>-->
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
						<el-form-item label="名称">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="权限">
							<el-tree ref="tree" :data="form.data.navs" show-checkbox node-key="id" :default-checked-keys="form.ids" :default-expand-all="true" :props="defaultProps">
							</el-tree>
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

						navs: [{
							displayName: '',
							icon: '',
							id: '',
							name: '',
							parent_id: '',
							selected: '',
							sortNumber: '',
							url: '',
							children: [{
								displayName: '',
								icon: '',
								id: '',
								name: '',
								parent_id: '',
								selected: '',
								sortNumber: '',
								url: '',
							}]

						}]
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
				this.form.title = "创建用户";
				this.form.disabled = false;
				this.form.data.name = '';

				this.$get('admin/role/get', {
						id: 1
					}).then2(function(response) {
						me.form.data = response.data.data;
						me.form.data.id = '';
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
				me.form.ids = [];
				this.form.data.id = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改角色管理";
				me.form.visible = true;
				me.form.ids = [];
				this.$get('admin/role/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						for(var i = 0; i < me.form.data.navs.length; i++) {
							/*if(me.form.data.navs[i].selected == true) {
								//me.form.data.navs[i].id
								me.form.ids[i] = response.data.data.navs[i].id;
							} else {
								me.form.ids[i] = '';
							}*/
							if(me.form.data.navs[i].children != undefined && me.form.data.navs[i].children != '') {
								for(var j = 0; j < me.form.data.navs[i].children.length; j++) {
									if(me.form.data.navs[i].children[j].selected == true) {
										me.form.ids.push(me.form.data.navs[i].children[j].id);
									}
								}
							} else if(me.form.data.navs[i].selected == true) {
								me.form.ids.push(me.form.data.navs[i].id);
							}
						}
						//alert(me.form.ids);
						//alert(JSON.stringify(me.form.data));
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
					this.$post('admin/role/batchDelete', {
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

			},
			handleSubmit() {
				var me = this;
				var ids = [];
				var treeNodes = [];
				var treeKeys = [];
				//alert("getCheckedNodes," + JSON.stringify(this.$refs.tree.getCheckedNodes(true)));
				//alert("getCheckedKeys," + JSON.stringify(this.$refs.tree.getCheckedKeys(true)));
				//alert("getHalfCheckedNodes," + JSON.stringify(this.$refs.tree.getHalfCheckedNodes()));
				//alert("getHalfCheckedKeys," + JSON.stringify(this.$refs.tree.getHalfCheckedKeys()));
				//alert("getCurrentKey," + JSON.stringify(this.$refs.tree.getCurrentKey()));
				//alert("getCurrentNode," + JSON.stringify(this.$refs.tree.getCurrentNode()));
				treeNodes = this.$refs.tree.getCheckedNodes(true);
				treeKeys = this.$refs.tree.getCheckedKeys();

				/**
				 * 子节点id
				 */
				for(var i = 0; i < treeKeys.length; i++) {
					//alert(treeKeys);
					ids[i] = treeKeys[i];
				}
				/**
				 * 选中子节点的父节点id
				 */
				for(var i = 0; i < treeNodes.length; i++) {
					ids.push(treeNodes[i].parent_id);
				}
				ids = JSON.stringify(ids);

				/*let node = me.form.data.navs;
				let ok = false;
				let result; // 包含你说的那个子节点的父节点*/
				/*while(!ok) {
					let item = node.shift();
					if(item.id == id) {
						result = item;
						ok = true;
					} else if(item.children && item.children.length > 0) {
						node = node.concat(item.children);
					}
				}*/
				//alert(JSON.stringify(node.shift()).id);

				/*if(me.form.data.navs[i].children != undefined && me.form.data.navs[i].children != '') {
					for(var i = 0; i < me.form.data.navs.children.length; i++) {
						for(var j = 0; j < ids.length; j++) {
							if(me.form.data.navs.children[i].id == ids[j]) {
								me.form.data.navs.children[i].selected = true;
							} else {
								me.form.data.navs.children[i].selected = false;
							}
						}
					}
				}*/
				for(var i = 0; i < me.form.data.navs.length; i++) {
					if(ids.indexOf(me.form.data.navs[i].id)) {
						me.form.data.navs[i].selected = true;
					} else {
						me.form.data.navs[i].selected = false;
					}
					if(me.form.data.navs[i].children != undefined && me.form.data.navs[i].children != '') {
						for(var j = 0; j < me.form.data.navs[i].children.length; j++) {
							if(ids.indexOf(me.form.data.navs[i].children[j].id)) {
								me.form.data.navs[i].children[j].selected = true;
							} else {
								me.form.data.navs[i].children[j].selected = false;
							}
						}
					}
				}

				/*for(var i = 0; i < me.form.data.navs.length; i++) {
					for(var j = 0; j < ids.length; j++) {
						if(me.form.data.navs[i].id == ids[j]) {
							me.form.data.navs[i].selected = true;
						} else {
							me.form.data.navs[i].selected = false;
						}
					}
				}*/
				var data = me.form.data;
				/*for(var i = 0; i < ids.length-1; i++){
					ids[i] = '"' + ids[i] + '"';
				}*/
				data.ids = ids;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				
				if(data.ids == "[]") {
					me.$notify({
						title: '警告',
						message: '请输勾选权限！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/role/update', data).then2(function(response) {
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
					this.$post('admin/role/create', data).then2(function(response) {
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
					me.searchStr += ' 角色名称： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				var me = this;
				this.$get("admin/role/many", key)
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
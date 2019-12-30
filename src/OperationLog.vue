<template scoped>
	<div>
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<!-- <span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span> -->
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="userName" label="用户名" width="150">
						</el-table-column>
						<el-table-column prop="ip" label="ip" width="200">
						</el-table-column>
						<el-table-column prop="description" label="描述" width="200">
						</el-table-column>
						<el-table-column prop="created_at" label="时间" width="200">
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
							</el-button-group>
						</el-col>
						<el-col :span="16" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
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
			
			handleSearch() {
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				
				var me = this;
				this.$get("admin/operationLog/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].disabled == 0) {
								me.table.items[i].disabled = "启用";
							} else if(me.table.items[i].disabled == 1) {
								me.table.items[i].disabled = "不启用";
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
<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询积分" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="" v-model="search.data.userName" :disabled="!search.useUserName" class="bottom20" style="width: 87%;">
						<el-checkbox label="用户名" slot="prepend" v-model="search.useUserName"></el-checkbox>
					</el-input>
				</el-col>

				<!--<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="是否禁用" v-model="search.useDisabled" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%" v-model="search.data.disabled" placeholder="" :disabled="!search.useDisabled">
								<el-option label="启用" value="0"></el-option>
								<el-option label="禁用" value="1"></el-option>
							</el-select>
						</el-col>
					</el-row>
				</el-col>-->
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
    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="总积分数" v-bind:part1="IntegralNum" back="#409EFF"></bc-statbox></el-col>
    </el-row>
		<el-row :gutter="20">
			<!-- <el-col :span="4">
        <bc-panel title="查询用户" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>
          <el-input placeholder="请输入用户名" v-model="search.data.userName" :disabled="!search.useUserName" class="bottom20">
            <el-checkbox label="用户名" slot="prepend" v-model="search.useUserName"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入角色" v-model="search.data.roleName" :disabled="!search.useRoleName" class="bottom20">
            <el-checkbox label="角色" slot="prepend" v-model="search.useRoleName"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="是否禁用" v-model="search.useDisabled" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.disabled" placeholder="" :disabled="!search.useDisabled">
                <el-option label="启用" value="0"></el-option>
                <el-option label="禁用" value="1"></el-option>
              </el-select>
            </el-col>
          </el-row>
        </bc-panel>
      </el-col>-->
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="customerName" label="用户名">
						</el-table-column>
						<el-table-column prop="amount" label="积分数量">
						</el-table-column>
						<el-table-column prop="source" label="积分来源">
						</el-table-column>
						<el-table-column prop="type" label="类型">
						</el-table-column>
						<el-table-column prop="created_at" label="日期">
						</el-table-column>
						<el-table-column prop="note" label="备注">
						</el-table-column>
						<!--<el-table-column label="查看" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">查看</el-button>
							</template>
						</el-table-column>-->
					</el-table>
					<el-row slot="footer">
						<!--<el-col :span="8">
							<el-button-group>
								<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>-->
						<el-col :span="24" style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="用户名">
							<el-input v-model="form.data.userName" placeholder="用户名"></el-input>
						</el-form-item>
						<el-form-item label="昵称">
							<el-input v-model="form.data.nickName" placeholder="昵称"></el-input>
						</el-form-item>
						<el-form-item label="密码">
							<el-input type="password" v-model="form.data.password" placeholder="密码"></el-input>
						</el-form-item>
						<el-form-item label="分配角色">
							<el-select v-model="form.data.role_id" placeholder="请选择" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="超级管理员" value="1"></el-option>
								<el-option label="订单管理" value="2"></el-option>
								<el-option label="财务管理" value="4"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="是否启用">
							<el-select v-model="form.data.disabled" placeholder="请选择" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="不启用" value="1"></el-option>
								<el-option label="启用" value="0"></el-option>
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
						amount: '',
						id: '',
						created_at: '',
						customer_id: '',
						cycleNum: '',
						note: '',
						relate_id: '',
						type:'',
						source:''
					}, ],
					selections: [],
				},
				IntegralNum:0,
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						amount: '',
						id: '',
						created_at: '',
						customer_id: '',
						cycleNum: '',
						note: '',
						relate_id: '',
						type:'',
						source:''
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
			handleCreate() {
				this.form.title = "创建用户";
				this.form.disabled = false;
				this.form.data.disabled = '';
				this.form.data.id = '';
				this.form.data.nickName = '';
				this.form.data.password = '';
				this.form.data.role_id = '';
				this.form.data.userName = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改用户管理";
				me.form.visible = true;

				this.$get('admin/user/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(me.form.data.role_id == 1) {
							me.form.data.role_id = "超级管理员";
						} else if(me.form.data.role_id == 2) {
							me.form.data.role_id = "订单管理";
						} else if(me.form.data.role_id == 4) {
							me.form.data.role_id = "财务管理";
						}
						if(me.form.data.disabled == 0) {
							me.form.data.disabled = "启用";
						} else if(me.form.data.disabled == 1) {
							me.form.data.disabled = "不启用";
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
					me.$post('admin/user/deleteBatch', {
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
				if(me.form.data.role_id == "超级管理员") {
					me.form.data.role_id = 1;
				} else if(me.form.data.role_id == "订单管理") {
					me.form.data.role_id = 2;
				} else if(me.form.data.role_id == "财务管理") {
					me.form.data.role_id = 4;
				}
				if(me.form.data.disabled == "启用") {
					me.form.data.disabled = 0;
				} else if(me.form.data.disabled == "不启用") {
					me.form.data.disabled = 1;
				}
				data.roleId = data.role_id;
				if(!data.userName) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/user/update', data).then2(function(response) {
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
					this.$post('admin/user/create', data).then2(function(response) {
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
				this.IntegralNum = 0;
				
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.count = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useUserName && this.search.data.userName) {
					key.userName = this.search.data.userName;
					me.searchStr += ' 用户名： ' + key.userName;
					if(key.userName != '') {
						me.searchStr += '；';
					}
				}
				/*if(this.search.useRoleName && this.search.data.roleName) {
					key.roleName = this.search.data.roleName;
				}
				if(this.search.useDisabled && this.search.data.disabled) {
					key.disabled = this.search.data.disabled;
				}*/
				var me = this;
				this.$get("admin/customer/getPoint", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
						for(var i =0;i<me.table.items.length;i++){
							if(me.table.items[i].type == 1){
							me.table.items[i].type="获取";
							}else if(me.table.items[i].type == 2){
							me.table.items[i].type="扣除";
							}
							
							if(me.table.items[i].source==9){
								me.table.items[i].source="每日签到"
							}else if(me.table.items[i].source==11){
								me.table.items[i].source="评价商品";
							}else if(me.table.items[i].source==5){
								me.table.items[i].source="完善个人信息";
							}else if(me.table.items[i].source==3){
								me.table.items[i].source="新用户注册";
							}else if(me.table.items[i].source==13){
	                        	me.table.items[i].source="点击广告";
	                        }else if(me.table.items[i].source==7){
	                        	me.table.items[i].source="邀请好友注册成功一次";
	                        }else if(me.table.items[i].source==15){
	                        	me.table.items[i].source="分享活动链接";
	                        }else if(me.table.items[i].source==17){
	                        	me.table.items[i].source="购买特产类商品第三方支付";
	                        }else if(me.table.items[i].source==20){
	                        	me.table.items[i].source="购买特产类商品钱包支付";
	                        }else if(me.table.items[i].source==23){
	                        	me.table.items[i].source="购买旅游类产品第三方支付";
	                        }else if(me.table.items[i].source==26){
	                        	me.table.items[i].source="购买旅游类产品钱包支付";
	                        }else if(me.table.items[i].source==31){
	                        	me.table.items[i].source="抽奖";
	                        }else if(me.table.items[i].source==29){
	                        	me.table.items[i].source="取消订单";
	                        }else if(me.table.items[i].source==40){
	                        	me.table.items[i].source="退货";
	                        }else if(me.table.items[i].source==41){
	                        	me.table.items[i].source="退款";
	                        }
								
							me.IntegralNum = me.table.items[i].amount+me.IntegralNum;
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
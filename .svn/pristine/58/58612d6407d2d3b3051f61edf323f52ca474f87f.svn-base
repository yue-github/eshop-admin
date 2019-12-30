<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询个人会员" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: left; margin-left: 10px;">
							<el-checkbox label="性别" v-model="search.useSex" style="margin-top: 5px"></el-checkbox>
						</el-col>

						<el-col :span="16">
							<el-select style="width: 100%; margin-left: -20px;" v-model="search.data.sex" placeholder="请选择性别" :disabled="!search.useSex">
								<el-option label="未知" value="0"></el-option>
								<el-option label="男" value="1"></el-option>
								<el-option label="女" value="2"></el-option>
							</el-select>
						</el-col>
					</el-row>

				</el-col>

				<el-col :span="12">

					<el-input placeholder="请输入昵称关键字" v-model="search.data.nickName" :disabled="!search.useNickName" class="bottom20 input-size">
						<el-checkbox label="昵称" slot="prepend" v-model="search.useNickName"></el-checkbox>
					</el-input>

					<el-input placeholder="请输入手机号码" v-model="search.data.mobilePhone" :disabled="!search.usePhone" class="bottom20 input-size">
						<el-checkbox label="手机" slot="prepend" v-model="search.usePhone"></el-checkbox>
					</el-input>

				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<!-- <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row> -->
		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="name" label="姓名">
						</el-table-column>
						<el-table-column prop="sex" label="性别">
						</el-table-column>
						<el-table-column prop="nickName" label="昵称">
						</el-table-column>
						<el-table-column prop="mobilePhone" label="电话号码">
						</el-table-column>
						<el-table-column prop="points" label="可用积分">
						</el-table-column>
						<el-table-column prop="golds" label="可用积分">
						</el-table-column>
						<el-table-column prop="grade" label="会员等级">
						</el-table-column>
						<el-table-column prop="note" label="个人简介">
						</el-table-column>
						<el-table-column label="编辑" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<el-col style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>
				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="姓名">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="昵称">
							<el-input v-model="form.data.nickName" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="性别">
							<el-select v-model="form.data.gender" placeholder="是否需要禁用用户" style="width: 100%" :disabled="form.disabled">
								<el-option label="请选择" value="0"></el-option>
								<el-option label="男" value="1"></el-option>
								<el-option label="女" value="2"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="出生日期">
							<el-date-picker v-model="form.data.dob" type="date" placeholder="选择日期" value-format="yyyy-MM-dd" :disabled="form.disabled">
							</el-date-picker>
						</el-form-item>
						<el-form-item label="会员等级">
							<el-select v-model="form.data.grade" placeholder="请选择" style="width: 100%">
								<el-option v-for="item in grade" :key="item.id" :label="item.name" :value="item.name">
								</el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="是否禁用">
							<el-select v-model="form.data.disable" placeholder="是否需要禁用用户" style="width: 100%">
								<el-option label="请选择" value="-1"></el-option>
								<el-option label="是" value="1"></el-option>
								<el-option label="否" value="0"></el-option>
							</el-select>
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
						name: '1',
						sex: 'haha',
						nickName: 'alisda',
						mobilePhone: '13568945682',
						note: '舒服'
					}, ],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: true,
					data: {
						balancePwd: '',
						created_at: '',
						disable: '',
						dob: '',
						email: '',
						gender: '',
						headImg: '',
						homePhone: '',
						id: '',
						isMobileVerified: '',
						login: '',
						mobilePhone: '',
						name: '',
						note: '',
						occupation: '',
						password: '',
						qqOpenId: '',
						unionid: '',
						updated_at: '',
						weiXinOpenId: '',
						workPhone: ''
					}
				},
				grade: {
					name: ''
				},
				search: {
					visible: false,
					useName: true,
					useSex: true,
					useNickName: true,
					usePhone: true,
					data: {
						name: '',
						sex: '',
						nickName: '',
						mobilePhone: ''
					}
				}
			}
		},
		methods: {
			getNowFormatDate(date) {
				var seperator1 = "-";
				var year = date.getFullYear();
				var month = date.getMonth() + 1;
				var strDate = date.getDate();
				if(month >= 1 && month <= 9) {
					month = "0" + month;
				}
				if(strDate >= 0 && strDate <= 9) {
					strDate = "0" + strDate;
				}
				var currentdate = year + seperator1 + month + seperator1 + strDate;
				return currentdate;
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
			// handleCreate() {
			//   this.form.title = "创建用户";
			//   this.form.disabled = false;
			//   this.form.data.name = '';
			//   this.form.data.sex = 'unkown';
			//   this.form.data.role = 'query';
			//   this.form.data.mobilePhone = '';
			//   this.form.data.address = '';
			//   this.form.visible = true;
			// },
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改会员";
				me.form.visible = true;

				this.$get('admin/customer/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.customer;
						if(me.form.data.disable == 1) {
							me.form.data.disable = '是';
						} else if(me.form.data.disable == 0) {
							me.form.data.disable = '否';
						}
						if(me.form.data.gender == 1) {
							me.form.data.gender = '男';
						} else if(me.form.data.gender == 0) {
							me.form.data.gender = '女';
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

			},
			handleSubmit() {
				var me = this;
				var data = me.form.data;
				var a = /^(\d{4})-(\d{2})-(\d{2})$/;
				if(!a.test(data.dob)) {
					data.dob = me.getNowFormatDate(data.dob);
				}
				if(me.form.data.disable == '是') {
					me.form.data.disable = 1;
				} else if(me.form.data.disable == '否') {
					me.form.data.disable = 0;
				}
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/customer/update', data).then2(function(response) {
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
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += ' 名称： ' + key.name;
					if(key.name != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useSex && this.search.data.sex) {
					key.gender = this.search.data.sex;
					me.searchStr += ' 性别： ' + (key.gender == "0"?"未知":"")+(key.gender == "1"?"男":"")+(key.gender =="2"?"女":"");
					if(key.gender != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.useNickName && this.search.data.nickName) {
					key.nickName = this.search.data.nickName;
					me.searchStr += ' 昵称： ' + key.nickName;
					if(key.nickName != '') {
						me.searchStr += ';';
					}
				}
				if(this.search.usePhone && this.search.data.mobilePhone) {
					key.phone = this.search.data.mobilePhone;
					me.searchStr += ' 手机号： ' + key.phone ;
					if(key.phone != '') {
						me.searchStr += ';';
					}
				}

				var me = this;
				this.$get("admin/customer/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						for(var i = 0; i < response.data.data.length; i++) {
							if(response.data.data[i].gender == 1) {
								me.table.items[i].sex = '男';
							} if(response.data.data[i].gender == 2) {
								me.table.items[i].sex = '女';
							}if(response.data.data[i].gender == 0) {
								me.table.items[i].sex = '未知';
							}
						}
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
			},
			handleGrade() {
				var key = {};
				var me = this;
				key.count = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				this.$get('admin/customer/getGrade', key).then2(function(response) {
						me.grade = response.data.data;
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
		},
		mounted: function() {
			this.handleSearch();
			this.handleGrade();
		}
	}
</script>
<style>
	.input-size {
		width: 87%
	}
</style>
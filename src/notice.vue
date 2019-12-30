<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询用户" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<!--<el-col :span="12">
					<el-input placeholder="请输入客户名" v-model="search.data.name" class="bottom20 input-size">
						<el-checkbox label="客户名" slot="prepend" v-model="search.name"></el-checkbox>
					</el-input>
				</el-col>-->

				<el-col :span="12">
					<el-input placeholder="请输入手机号码" v-model="search.data.mobilePhone" class="bottom20 input-size">
						<el-checkbox label="手机号码" slot="prepend" v-model="search.mobilePhone"></el-checkbox>
					</el-input>
				</el-col>
				<el-col :span="12">
					<el-input placeholder="请输入邮箱" v-model="search.data.email" class="bottom20 input-size">
						<el-checkbox label="邮箱" slot="prepend" v-model="search.email"></el-checkbox>
					</el-input>
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click="handleSearch()">查询</el-button>
			</div>
		</el-dialog>
		<el-row :gutter="24">
			<el-col :span="24">
				<bc-panel title="消息发送" icon="icon-grid icons">
					<el-form ref="form" :model="form.data" label-width="100px" style="text-align: center;">
						<el-row :gutter="18" type="flex" justify="center">
							<el-col :span="18">
								<el-form-item label="发送内容">
									<el-input type="textarea" v-model="form.data.msg_content" :rows="5" placeholder="请填写消息内容"></el-input>
								</el-form-item>
								<el-form-item label="消息等级">
									<el-select v-model="form.data.msg_grade" placeholder="请选择消息等级" style="width: 100%;">
										<el-option label="通知" value="1"></el-option>
										<el-option label="重要" value="2"></el-option>
										<el-option label="紧急" value="3"></el-option>
									</el-select>
								</el-form-item>
								<el-form-item label="发送对象" style="text-align: left;">
									<el-radio-group v-model="selectObj" @change="handleChaneSelectObj">
										<el-radio label="1" boder>全部客户</el-radio>
										<!--<el-radio label="2" boder>部分客户</el-radio>-->
										<el-radio label="3" boder>微信客户</el-radio>
										<el-radio label="4" boder>短信客户</el-radio>
										<el-radio label="5" boder>成交客户</el-radio>
									</el-radio-group>
								</el-form-item>
								<el-form-item label="选择发送对象" v-if="panelShow">
									<bc-panel style="padding: 0;">
										<label style="float: left; line-height: 0;">{{searchStr}}</label>
										<el-button type="primary" @click="search.visible = true" style="float: right;margin-top: -20px;">查询</el-button>
										<el-table ref="multipleTable" row-key="id" :data="table.items" border @select-all="handleSelectionAll" @selection-change="handleSelectionChange" style="width: 100%" height="390">
											<el-table-column type="selection" width="55" :reserve-selection="true">
											</el-table-column>
											<!--<el-table-column prop="name" label="客户名">
											</el-table-column>-->
											<el-table-column prop="nickName" label="昵称">
											</el-table-column>
											<el-table-column prop="mobilePhone" label="手机号码">
											</el-table-column>
											<el-table-column prop="email" label="邮箱">
											</el-table-column>
										</el-table>
										<el-row slot="footer">
											<el-col :span="24" style="text-align: right">
												<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
												</el-pagination>
											</el-col>
										</el-row>
									</bc-panel>
								</el-form-item>
								<el-form-item style="text-align: right">
									<el-button type="primary" @click="formValidate()">发送</el-button>
								</el-form-item>
							</el-col>
						</el-row>
					</el-form>
				</bc-panel>
			</el-col>
		</el-row>
		<el-dialog title="发送方式" :visible.sync="dialogSendNoticeVisible" center style="width: 40%; margin: 0 auto; text-align: center;">
			<el-row :gutter="24" type="flex" justify="center">
				<el-col :span="24">
					<div style="padding: 10px 0;">
						<el-checkbox-group v-model="checked" @change="handleSendType">
							<el-checkbox :label="1" :key="sms" border>短信发送</el-checkbox>
							<el-checkbox :label="2" :key="notice" border>消息通知</el-checkbox>
						</el-checkbox-group>
					</div>
					<el-button type="primary" @click="handleSend()">消息通知</el-button>
				</el-col>
			</el-row>
		</el-dialog>
	</div>
</template>

<script>
	module.exports = {
		data: function() {
			return {
				dialogSendNoticeVisible: false,
				searchStr: '',
				panelShow: false,
				selectObj: '',
				checked: [],
				sendSMS: false,
				sendNotice: false,
				doubleSend: false,
				checkVal: [],
				tabIndex: 0,
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [],
					selections: [],
				},
				form: {
					title: '',
					disabled: false,
					data: {
						id: '',
						customer_ids: '',
						msg_content: '',
						msg_grade: '',
						created_at: '',
						isRead: '',
					}
				},
				search: {
					visible: false,
					name: true,
					mobilePhone: true,
					email: true,
					data: {
						name: '',
						mobilePhone: '',
						email: ''
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
					name: '',
					note: '',
					parent_id: '',
					sortNumber: '',
					id: ''
				}],
				customer: [],
				// 列表全选与否
				allSelect: false
			}
		},
		methods: {
			handleSelectionAll(val) {
				var me = this;
				me.allSelect = me.allSelect == true ? false : true;
				if(me.allSelect == false) {
					me.table.selections = "";
					me.$refs.multipleTable.clearSelection();
				} else {
					if(me.selectObj == 5) {
						var key = {};
						key.length = me.table.total;
						key.offset = 0;
						this.$get("admin/customer/getTransactionCustomer", key)
							.then2(function(response) {
								me.table.selections = response.data.data;
								me.search.visible = false;
							}).catch(function(err) { 
								console.log(err);
							});
					} else {
						var key = {};
						key.length = me.table.total;
						key.offset = 0;
						this.$get("admin/customer/many", key)
							.then2(function(response) {
								me.table.selections = response.data.data;
								me.search.visible = false;
							}).catch(function(err) { 
								console.log(err);
							});
					}
				}
			},
			handleChaneSelectObj(val) {
				var me = this;
				if(me.allSelect == true) {
					me.allSelect = false;
					me.$refs.multipleTable.clearSelection();
				}
				var key = {};
				key.length = me.table.pageCount;
				key.offset = 0;
				me.tabIndex = val;

				if(val == 1) {
					this.panelShow = false;
					key.length = me.table.total;
					me.handleChaneSelectObjAjax(key);
				} else if(val == 2) {
					this.panelShow = true;
					me.handleChaneSelectObjAjax(key);
				} else if(val == 3) {
					this.panelShow = true;
					key.weiXinOpenId = "true";
					me.handleChaneSelectObjAjax(key);
				} else if(val == 4) {
					this.panelShow = true;
					key.mobilePhone = "true";
					me.handleChaneSelectObjAjax(key);
				} else if(val == 5) {
					this.panelShow = true;
					this.$get("admin/customer/getTransactionCustomer", key)
						.then2(function(response) {
							me.customer = response.data.data;
							me.table.items = response.data.data;
							me.table.total = response.data.totalRow;
							for(var i = 0; i < me.table.items.length; i++) {
								me.table.items[i].nickName = me.table.items[i].receiveName;
							}
						}).catch(function(err) { 
							console.log(err);
						});
				}
			},
			handleChaneSelectObjAjax(key) {
				var me = this;
				this.$get("admin/customer/many", key)
					.then2(function(response) {
						me.customer = response.data.data;
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
					}).catch(function(err) { 
						console.log(err);
					});
			},
			handleSelectionChange(val) {
				//有bug
				var me = this;
				if(me.allSelect == false){
					me.table.selections = val;
				}
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "修改记录";
				me.form.visible = true;

				this.$get('admin/category/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.category;
						me.form.data.parentName = row.parentName;
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			formValidate() {
				var me = this;
				var formData = me.form.data;
				if(!this.$trim(formData.msg_content)) {
					me.$alert("发送内容不能为空", "提示", {
						confirmButtonText: '确定',
					});
					return false;
				} else if(!formData.msg_grade) {
					me.$alert("请选择发送等级", "提示", {
						confirmButtonText: '确定',
					});
					return false;
				} else if(!me.selectObj) {
					console.log("select: " + !me.selectObj)
					me.$alert("请选择发送对象", "提示", {
						confirmButtonText: '确定',
					});
					return false;
				}

				me.dialogSendNoticeVisible = true;
			},
			handleSendType(val) {
				var me = this;
				me.checkVal = [];
				me.sendNotice = false;
				me.sendSMS = false;
				for(var i = 0; i < val.length; i++) {
					if(val[i] == 1) {
						me.checkVal.push(1);
						me.sendSMS = true;
					}
					if(val[i] == 2) {
						me.checkVal.push(1);
						me.sendNotice = true;
					}
				}
			},
			/**
			 * 消息通知发送
			 */
			handleSendNotice() {
				var me = this;
				var ids = [];
				var formData = me.form.data;

				if(me.selectObj == 1) {
					var data = me.customer;
					for(var i = 0; i < data.length; i++) {
						ids.push(data[i].id);
					}
					formData.customer_ids = JSON.stringify(ids);

				} else {
					var selections = me.table.selections;
					if(selections.length == 0) {
						me.$alert("请选择要发送的客户", "提示", {
							confirmButtonText: '确定',
						});
						return false;
						/*for(var i = 0; i < me.customer.length; i++) {
							ids.push(me.customer[i].id);
						}
						alert(ids.length);*/
					} else {
						for(var i = 0; i < selections.length; i++) {
							ids.push(selections[i].id);
						}
					}
					formData.customer_ids = JSON.stringify(ids);
				}
				this.$post('admin/notice/create', formData).then2(function(response) {
					me.$notify({
						title: '成功',
						message: '保存成功',
						type: 'success'
					});
					me.dialogSendNoticeVisible = false;
				}, function(response) {
					me.$notify.error({
						title: '错误',
						message: '保存失败',
					});
					me.dialogSendNoticeVisible = false;
				});

			},
			/**
			 * 短信通知发送
			 */
			handleSendSMS() {
				var me = this;
				var ids = [];
				var formData = me.form.data;

				me.formValidate();

				if(me.selectObj == 1) {
					var data = me.customer;
					for(var i = 0; i < data.length; i++) {
						ids.push(data[i].mobilePhone);
					}
					formData.customer_ids = JSON.stringify(ids);

				} else {
					var selections = me.table.selections;
					if(selections.length == 0) {
						me.$alert("请选择要发送的客户", "提示", {
							confirmButtonText: '确定',
						});
						return false;
					}
					for(var i = 0; i < selections.length; i++) {
						ids.push(selections[i].mobilePhone);
					}
					formData.customer_ids = JSON.stringify(ids);
				}

				this.$post('admin/notice/sendSMS', formData)
					.then2(function(response) {
						me.$notify({
							title: '成功',
							message: '保存成功',
							type: 'success'
						});
						me.dialogSendNoticeVisible = false;
					}, function(response) {
						me.$notify.error({
							title: '错误',
							message: '保存失败',
						});
						me.dialogSendNoticeVisible = false;
					});

			},
			handleSend() {
				var me = this;
				var value = 0;
				for(var i = 0; i < me.checkVal.length; i++) {
					value += me.checkVal[i];
				}
				if(value == 2) {
					//短信 + 消息通知
					me.handleSendSMS();
					me.handleSendNotice();
				} else if(value == 1) {
					if(me.sendSMS) {
						//短信
						me.handleSendSMS();
					} else if(me.sendNotice) {
						//消息通知	
						me.handleSendNotice();
					}
				}
			},
			handleSearch() {
				var key = {};
				var me = this;
				var val = me.tabIndex;
				var url = "admin/customer/many";
				me.searchStr = "搜索";
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;

				if(val == 3) {
					key.weiXinOpenId = "true";
				} else if(val == 4) {
					key.mobilePhone = "true";
				} else if(val == 5) {
					url = "admin/customer/getTransactionCustomer";
				}

				if(this.search.name && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += '"' + key.name + '"';
				}
				if(this.search.mobilePhone && this.search.data.mobilePhone) {
					key.phone = this.search.data.mobilePhone;
					if(key.name) {
						me.searchStr += ',';
					}
					me.searchStr += '"' + key.phone + '"';
				}
				if(this.search.email && this.search.data.email) {
					key.email = this.search.data.email;
					if(key.name || key.phone) {
						me.searchStr += ',';
					}
					me.searchStr += '"' + key.email + '"';
				}

				this.$get(url, key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
						if(me.allSelect == true) {
							me.table.items.forEach(row => {
								me.$refs.multipleTable.toggleRowSelection(row, true);

							});
						}
					}).catch(function(err) { 
						console.log(err);
					});
			},
			handleSizeChange(val) {
				this.table.pageCount = val;
				this.handleSearch();
			},
			handleCurrentChange(val) {
				this.table.pageIndex = val;
				this.handleSearch();
			}
			
		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
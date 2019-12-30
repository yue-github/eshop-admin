<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询手续费" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-col :span="5" style="text-align: center">
						<el-checkbox label="支付平台" v-model="search.useName" style="margin-top: 5px"></el-checkbox>
					</el-col>
					<el-col :span="16">
						<el-select style="width: 100%" v-model="search.data.payType" placeholder="请选择支付平台" :disabled="!search.useName">
							<el-option label="微信支付" value="1"></el-option>
							<el-option label="支付宝" value="2"></el-option>
							<el-option label="银联支付(借记卡)" value="3"></el-option>
							<el-option label="钱包支付" value="4"></el-option>
						</el-select>
					</el-col>
				</el-col>

				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="状态" v-model="search.useDisable" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%" v-model="search.data.enable" placeholder="请选择是否禁用" :disabled="!search.useDisable">
								<el-option label="不启用" value="2"></el-option>
								<el-option label="启用" value="1"></el-option>
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
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						<el-table-column prop="payType" label="支付平台">
						</el-table-column>
						<el-table-column prop="rate" label="手续费(%)">
						</el-table-column>
						<el-table-column prop="enable" label="是否可用">
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
						<el-form-item label="支付平台">
							<el-select v-model="form.data.payType" placeholder="请选择支付平台" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="微信支付" value="1"></el-option>
								<el-option label="支付宝" value="2"></el-option>
								<el-option label="银联支付(借记卡)" value="3"></el-option>
								<el-option label="钱包支付" value="4"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="手续费比例(%)">
							<el-input-number v-model="form.data.rate" :disabled="form.disabled" controls-position="right" style="width: 100%;">
							</el-input-number>
							
						</el-form-item>
						<el-form-item label="是否启用">
							<el-select v-model="form.data.enable" style="width: 100%">
								<el-option label="请选择" value="unkown"></el-option>
								<el-option label="启用" value="1"></el-option>
								<el-option label="不启用" value="2"></el-option>
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
						enable: 'haha',
						payType: '未知',
						rate: '正常',
					}],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						enable: '',
						id: '',
						payType: '',
						rate: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					useSex: true,
					useDisable: true,
					usePhone: true,
					data: {
						payType: '',
						enable: ''
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
				this.form.title = "手续费信息";
				this.form.disabled = false;
				this.form.data.enable = '';
				this.form.data.id = '';
				this.form.data.payType = '';
				this.form.data.rate = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.title = "手续费信息";
				me.form.visible = true;

				this.$get('admin/payRate/get', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.data;
						if(me.form.data.payType == 1) {
							me.form.data.payType = "微信支付";
						} else if(me.form.data.payType == 2) {
							me.form.data.payType = "支付宝";
						} else if(me.form.data.payType == 3) {
							me.form.data.payType = "银联支付";
						} else if(me.form.data.payType == 4) {
							me.form.data.payType = "钱包支付";
						}
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
					this.$post('admin/payRate/batchDelete', {
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
					me.$post('admin/payRate/batchDelete', {
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
				if(me.form.data.payType == "微信支付") {
					me.form.data.payType = 1;
				} else if(me.form.data.payType == "支付宝") {
					me.form.data.payType = 2;
				} else if(me.form.data.payType == "银联支付") {
					me.form.data.payType = 3;
				} else if(me.form.data.payType == "钱包支付") {
					me.form.data.payType = 4;
				}
				if(me.form.data.enable == "启用") {
					me.form.data.enable = 1;
				} else if(me.form.data.enable == "不启用") {
					me.form.data.enable = 2;
				}
				if(data.payType == "unkown" || !data.payType) {
					me.$notify({
						title: '警告',
						message: '请选择支付平台！',
						type: 'warning'
					});
					return false;
				}
				if(!data.rate) {
					me.$notify({
						title: '警告',
						message: '请输入手续费比例！',
						type: 'warning'
					});
					return false;
				}
				if(data.enable == "unkown" || !data.enable) {
					me.$notify({
						title: '警告',
						message: '请选择是否启用！',
						type: 'warning'
					});
					return false;
				}
				
				if(data.id) {
					this.$post('admin/payRate/update', data).then2(function(response) {
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
					this.$post('admin/payRate/create', data).then2(function(response) {
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

				if(this.search.useName && this.search.data.payType) {
					key.payType = this.search.data.payType;
					var temp;
					switch(parseInt(key.payType)){
						case 1:
							temp = "微信支付";
							break;
						case 2:
							temp ="支付宝";
							break;
						case 3:
							temp = "银联支付（借记卡）";
							break;
						case 4:
							temp ="钱包支付";
							break;
						default:
							temp = key.payType;
					}
					me.searchStr += ' 支付平台： ' + temp;
					if(key.payType != '') {
						me.searchStr += '；';
					}
				}

				if(this.search.useDisable && this.search.data.enable) {
					key.enable = this.search.data.enable;
					me.searchStr += ' 状态： ' + (key.enable=="1"?"启用":"不启用");
					if(key.enable != '') {
						me.searchStr += '；';
					}
				}

				if(this.search.useCustomerPhone && this.search.data.customerPhone) {
					key.customerPhone = this.search.data.customerPhone;
					me.searchStr += '"' + key.customerPhone + '"';
					if(key.customerPhone != '') {
						me.searchStr += ',';
					}
				}

				if(this.search.useEvent && this.search.data.event) {
					key.event = this.search.data.event;
					me.searchStr += '"' + key.event + '"';
					if(key.event != '') {
						me.searchStr += ',';
					}
				}

				if(this.search.useMoneyMoreThan && this.search.data.moneyMoreThan) {
					key.moneyMoreThan = this.search.data.moneyMoreThan;
					me.searchStr += '"' + key.moneyMoreThan + '"';
					if(key.moneyMoreThan != '') {
						me.searchStr += ',';
					}
				}

				if(this.search.useMoneyLessThan && this.search.data.moneyLessThan) {
					key.moneyLessThan = this.search.data.moneyLessThan;
					me.searchStr += '"' + key.moneyLessThan + '"';
					if(key.moneyLessThan != '') {
						me.searchStr += ',';
					}
				}

				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.$filter(this.search.data.startTime, 'yyyy-mm-dd', '1');
					me.searchStr += '"' + key.startTime + '"';
					if(key.startTime != '') {
						me.searchStr += ',';
					}
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.$filter(this.search.data.endTime, 'yyyy-mm-dd', '1');
					me.searchStr += '"' + key.endTime + '"';
					if(key.endTime != '') {
						me.searchStr += ',';
					}
				}
				if(this.search.useName && this.search.data.payType) {
					switch(this.search.data.payType) {
						case "微信支付":
							key.payType = 1;
							break;
						case "支付宝":
							key.payType = 2;
							break;
						case "银联支付":
							key.payType = 3;
							break;
						case "钱包支付":
							key.payType = 4;
							break;
						default:
							key.payType = this.search.data.payType;
					}
				}
				if(this.search.useDisable && this.search.data.enable) {
					switch(this.search.data.enable) {
						case "启用":
							key.enable = 1;
							break;
						case "不启用":
							key.enable = 2;
							break;
						default:
							key.enable = this.search.data.enable
					}
				}
				var me = this;
				this.$get("admin/payRate/many", key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].payType == 1) {
								me.table.items[i].payType = "微信支付";
							} else if(me.table.items[i].payType == 2) {
								me.table.items[i].payType = "支付宝";
							} else if(me.table.items[i].payType == 3) {
								me.table.items[i].payType = "银联支付";
							} else if(me.table.items[i].payType == 4) {
								me.table.items[i].payType = "钱包支付";
							}
							if(me.table.items[i].enable == 1) {
								me.table.items[i].enable = "启用";
							} else if(me.table.items[i].enable == 2) {
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
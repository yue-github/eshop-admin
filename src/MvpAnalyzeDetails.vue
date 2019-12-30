<template scoped>
	<div>
		<!--查询弹出框-->
		<!-- <el-dialog title="查询等级" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入等级" v-model="search.data.Name" :disabled="!search.useName" class="bottom20" style="width: 87%;">
						<el-checkbox label="等级" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click="handleSearch()">查询</el-button>
			</div>
		</el-dialog> -->

		<el-dialog title="订单详情" :visible.sync="dialogVisible" width="90%">
			<div>
				<el-card class="box-card">
					<div class="card-top-dialog">
						<el-row type="flex" class="row-bg">
							<el-col :span="15" class="card-top-dialog-one">商品信息</el-col>
							<el-col :span="3">数量</el-col>
							<el-col :span="3">单价</el-col>
							<el-col :span="3">总价</el-col>
						</el-row>
					</div>
					<div class="card-bottom-dailog card-dialog-center" >
						<el-row type="flex" align="middle">
							<el-col :span="15">
								<img src="./assets/default.jpg" alt="" />
								{{ orderDetailsData.shopName }}
							</el-col>
							<el-col :span="3">{{ orderDetailsData.totalProductCost }}</el-col>
							<el-col :span="3">{{ orderDetailsData.id }}</el-col>
							<el-col :span="3">div{{ orderDetailsData.totalProductCost * orderDetailsData.id }}</el-col>
						</el-row>
					</div>
					<div class="card-footer"></div>
				</el-card>
			</div>
			<div>
				<el-card class="box-card">
				  <div slot="header" class="clearfix">
				    <span class="user-data">下单用户信息</span>
				  </div>
				  <div>手机号码：{{orderDetailsData.preferredContactPhone}}</div>
				  <div>昵称：{{orderDetailsData.shopName}}</div>	
				</el-card>
				
			</div>
			<span slot="footer" class="dialog-footer"><el-button @click="dialogVisible = false">关 闭</el-button></span>
		</el-dialog>

		<el-row :gutter="20">
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;">{{ searchStr }}</span>
					<!-- <el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button> -->

					<div class="analyze-content">
						<div v-for="(item, index) in table.items" :key="index" class="">
							<el-card class="box-card">
								<div class="card-top">
									<el-row type="flex" class="row-bg">
										<el-col :span="5">下单时间：{{ item.created_at }}</el-col>
										<el-col :span="5">订单号：{{ item.order_code }}</el-col>
										<el-col :span="2">总价：{{ item.totalPayable }}</el-col>
										<el-col :span="10">运费：0</el-col>
										<el-col :span="2"><div class="span">操作</div></el-col>
									</el-row>
								</div>
								<div class="card-bottom">
									<el-row justify="center" align="middle">
										<el-col :span="6">
											<img src="./assets/default.jpg" alt="" />
											{{ item.shopName }}
										</el-col>
										<el-col :span="6">价格：{{ item.totalProductCost }}</el-col>
										<el-col :span="5">数量：{{ item.id }}</el-col>
										<el-col :span="5">金额{{ item.totalProductCost}}</el-col>
										<el-col :span="2"><el-button type="primary" @click="orderDetails(item)">订单详情</el-button></el-col>
									</el-row>
								</div>
								<div class="card-footer"></div>
							</el-card>
						</div>
					</div>

					<el-row slot="footer">
						<el-col :span="8">
							<el-button-group>
								<!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>-->
								<el-button type="success" size="small" @click="handleCreate()">
									<i class="icon-plus icons"></i>
									添加
								</el-button>
							</el-button-group>
						</el-col>
						<el-col :span="24" style="text-align: right">
							<el-pagination
								@size-change="handleSizeChange"
								@current-change="handleCurrentChange"
								:current-page="table.pageIndex"
								:page-sizes="[25, 50, 75, 100]"
								:page-size="25"
								layout="total, sizes, prev, pager, next, jumper"
								:total="table.total"
							></el-pagination>
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
			// 订单详情弹出框
			dialogVisible: false,
			// 订单详情数据
			orderDetailsData:{},
			searchStr: '',
			table: {
				total: 11,
				pageCount: 25,
				pageIndex: 1,
				items: [
					{
						id: '1',
						userName: 'haha',
						roleName: '未知',
						disabled: '正常'
					}
				],
				items1: [
					{
						id: '1',
						userName: 'haha',
						roleName: '未知',
						disabled: '正常'
					}
				],
				selections: []
			},
			gradename: [],
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
				},
				data1: {
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
				useName: true,
				useRoleName: true,
				useDisabled: true,
				data: {
					Name: '',
					roleName: '',
					disabled: ''
				}
			}
		};
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
		// 订单详情显示
		orderDetails(item) {
			this.dialogVisible = true;
			this.orderDetailsData = item
			console.log(item)
		},
		handleSearch() {
			//var data=this.search.data;
			var key = {};
			var me = this;
			me.searchStr = '搜索';
			key.count = this.table.pageCount;
			key.length = this.table.pageCount;
			key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
			key.status = 4;
			if (this.search.useName && this.search.data.Name) {
				key.name = this.search.data.Name;
				me.searchStr += ' 等级： ' + key.name;
				if (key.name != '') {
					me.searchStr += '；';
				}
			}
			if (this.search.useRoleName && this.search.data.roleName) {
				key.roleName = this.search.data.roleName;
				me.searchStr += '"' + key.roleName + '"';
				if (key.roleName != '') {
					me.searchStr += ',';
				}
			}
			if (this.search.useDisabled && this.search.data.disabled) {
				key.disabled = this.search.data.disabled;
				me.searchStr += '"' + key.disabled + '"';
				if (key.disabled != '') {
					me.searchStr += ',';
				}
			}

			var me = this;

			this.$get('admin/order/many', key)
				.then2(function(response) {
					me.table.items = response.data.data;
					me.table.total = response.data.totalRow;
					me.search.visible = false;
				})
				.catch(function(err) {
					console.log(err);
				});
		},
		handleBetton() {
			var me = this;
		}
	},
	mounted: function() {
		this.handleSearch();
		//this.handleBetton();
	}
};
</script>

<style scoped>
.box-card {
	margin: 20px 0;
}
.card-top {
	padding: 0 20px;
	height: 50px;
	line-height: 50px;
	margin-bottom: 20px;
	background-color: #dadada;
}
.card-top-dialog {
	padding: 0 20px;
	height: 50px;
	text-align: center;
	line-height: 50px;
	margin-bottom: 20px;
	background-color: #dadada;
}
.card-top-dialog-one {
	text-align: left;
}
.card-dialog-center div{
	padding-left: 15px;
}
.card-dialog-center img{
	padding-right: 15px;
}

.span {
	text-align: center;
	width: 100%;
}
.card-footer {
	height: 20px;
}
.card-bottom {
	text-align: center;
}
.card-bottom img {
	margin-right: 20px;
}
.card-bottom div {
	padding: 0 20px;
	height: 150px;
	display: flex;
	align-items: center;
}
.card-bottom-dailog{
	text-align: center;
}
..card-bottom-dailog img {
	margin-right: 20px;
}
.card-bottom-dailog div {
	
	height: 150px;
	display: flex;
	align-items: center;
}
.user-data{
	font-weight: bold;
}
</style>

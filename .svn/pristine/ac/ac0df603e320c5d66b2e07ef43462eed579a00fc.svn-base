<template scoped>
	<div>
		<!--查询弹出框-->
	    <el-dialog title="查询推荐位置" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
			    <el-col :span="12">
					<el-input placeholder="请输入标题" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="标题" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入排序" v-model="search.data.sortNumber" :disabled="!search.useSortNumber" class="bottom20 input-size">
						<el-checkbox label="排序" slot="prepend" v-model="search.useSortNumber"></el-checkbox>
					</el-input>
				
		      	</el-col>
		          
			   	<el-col :span="12">
			   		
				    <el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="推荐位置" v-model="search.useType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select v-model="search.data.type" style="width:100%"  placeholder="请选择推荐位置" :disabled="!search.useType">
								<el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
								</el-option>
							</el-select>
						</el-col>
					</el-row>
					
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center">
							<el-checkbox label="修改时间" v-model="search.useUpdated_at" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.updated_at" :disabled="!search.useUpdated_at" style="width:100%" type="date" placeholder="请选择修改时间" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
		        </el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
			     <el-button @click="search.visible = false">取 消</el-button>
			     <el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
	    </el-dialog>
		<!-- <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="位置总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="首页推荐" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="首页广告" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="内页推荐" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row> -->
		<el-row :gutter="20">
			<!--<el-col :span="4">
				<bc-panel title="查询推荐位置" icon="el-icon-search">
					<div slot="footer" style="text-align: right">
						<el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
					</div>
					<el-input placeholder="请输入标题" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
						<el-checkbox label="标题" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="推荐位置" v-model="search.useType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="17">
							<el-select placeholder="请选择订时间类型" :disabled="!search.useType">
							</el-select>
						</el-col>
					</el-row>
					<el-input placeholder="请输入排序" v-model="search.data.sortNumber" :disabled="!search.useSortNumber" class="bottom20">
						<el-checkbox label="排序" slot="prepend" v-model="search.useSortNumber"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="7" style="text-align: center">
							<el-checkbox label="修改时间" v-model="search.useUpdated_at" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="17">
							<el-date-picker v-model="search.data.updated_at" :disabled="!search.useUpdated_at" style="width:100%" type="date" placeholder="请选择修改时间" :picker-options="pickerOptions"></el-date-picker>
						</el-col>
					</el-row>
					<el-table-column label="操作" width="150">
						<template slot-scope="scope">
							<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
							<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button>
						</template>
					</el-table-column>
				</bc-panel>
			</el-col>-->
			<el-col :span="24">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column prop="name" label="标题">
						</el-table-column>
						<el-table-column prop="type" label="类型">
						</el-table-column>
						<el-table-column prop="sortNumber" label="排序">
						</el-table-column>
						<el-table-column prop="updated_at" label="修改时间">
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
								<!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 删除
								</el-button>-->
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
						<el-form-item label="推荐位置名称">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="类型">
							<el-select v-model="form.data.type" placeholder="请选择推荐类型" style="width: 100%">
								<el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
								</el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="排序">
							<el-input v-model="form.data.sortNumber" placeholder="请输入推荐排序"></el-input>
						</el-form-item>
						<el-form-item label="备注">
							<el-input v-model="form.data.note" placeholder="请输入推荐简介"></el-input>
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
						name: '',
						type: '',
						sortNumber: '',
						updated_at: '',
					}, ],
					selections: [],
				},
				types: {
					1: "首页轮播图",
					2: "首页推荐产品",
					3: "文章列表页轮播图",
					4: "首页推荐服务",
					5: "店铺主页轮播图",
					6: "周边游页面位置顶部",
					7: "周边游页面位置中间",
					8: "周边游页面位置底部",
					9: "优惠活动轮播图",
					10: "首页推荐分类",
					11: "周边游广告推荐",
					12: "首页广告",
					13: "优惠活动推荐产品",
					14: "移动端首页图标推荐",
					15: "移动端首页图标周边游",
					17: "移动端首页图标注册有礼",
				},
				options: [{
					value: 1,
					label: '首页轮播图'
				}, {
					value: 2,
					label: '首页推荐产品'
				}, {
					value: 3,
					label: '文章列表页轮播图'
				}, {
					value: 4,
					label: '首页推荐服务'
				}, {
					value: 5,
					label: '店铺主页轮播图'
				}, {
					value: 6,
					label: '店铺主页轮播图'
				}, {
					value: 7,
					label: '周边游页面位置顶部'
				}, {
					value: 8,
					label: '周边游页面位置中间'
				}, {
					value: 9,
					label: '周边游页面位置底部'
				}, {
					value: 10,
					label: '首页推荐分类'
				}, {
					value: 11,
					label: '周边游广告推荐'
				}, {
					value: 12,
					label: '首页广告'
				}, {
					value: 13,
					label: '优惠活动推荐产品'
				}, {
					value: 14,
					label: '移动端首页图标推荐'
				}, {
					value: 15,
					label: '移动端首页图标周边游'
				}, {
					value: 17,
					label: '移动端首页图标注册有礼'
				}],
				value:'',
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						id: '',
						name: '',
						note: '',
						sortNumber: '',
						type: '',
						mode: '',
						image: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					useType: true,
					useSortNumber: true,
					useUpdated_at: true,
					data: {
						name: '',
						type: '',
						sortNumber: '',
						updated_at: ''
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
				this.form.sortNumber = null;
				this.form.data.name = '';
				this.form.data.type = '';
				this.form.data.sortNumber='';
				this.form.data.note='';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.visible = true;

				this.$post('admin/recommend/getPosition', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.recommendPosition;
						//alert(me.form.data.type == me.options[1].value);
						/*for(var i = 0; i < me.options.length; i++){
							if(me.form.data.type == me.options[i].value){
								me.form.data.type = me.options[i].label;
							}
						}*/
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
					this.$post('admin/recommend/deletePosition', {
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
				var data = me.form.data;
				if(!data.name) {
					me.$notify({
						title: '警告',
						message: '请输入名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/recommend/updatePosition', data).then2(function(response) {
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
					this.$post('admin/recommend/createPosition', data).then2(function(response) {
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
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += ' 标题： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useType && this.search.data.type) {
					key.type = this.search.data.type;
					var temp;
					switch(parseInt(key.type)){
						case 1:
							temp = "首页轮播图";
							break;
						case 2:
							temp ="首页推荐产品";
							break;
						case 3:
							temp ="文章首页轮播图";
							break;
						case 4:
							temp = "首页推荐服务";
							break;
						case 5:
							temp = "店铺主页轮播图";
							break;
						case 6:
							temp = "店铺主页轮播图";
							break;
						case 7:
							temp = "周边游页面位置顶部";
							break;
						case 8:
							temp ="周边游页面位置中间";
							break;
						case 9:
							temp = "周边游页面位置尾部";
							break;
						case 10:
							temp = "首页推荐分类";
							break;
						case 11:
							temp = "周边游广告推荐";
							break;
						case 12:
							temp = "首页广告";
							break;
						case 13:
							temp = "优惠活动推荐产品";
							break;
						case 14:
							temp = "移动端首页图标推荐";
							break;
						case 15:
							temp = "移动端首页周边游";
							break;
						case 17:
							temp ="移动端首页注册有礼";
							break;
						default:
							temp = key.type;
					}
					me.searchStr += ' 推荐位置： ' +temp;
					if(key.type != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useSortNumber && this.search.data.sortNumber) {
					key.sortNumber = this.search.data.sortNumber;
					me.searchStr += ' 排序： ' +key.sortNumber;
					if(key.sortNumber != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useUpdated_at && this.search.data.updated_at) {
					key.updated_at = this.$filter(this.search.data.updated_at,'yyyy-mm-dd')
					me.searchStr += ' 修改时间： ' + key.updated_at ;
					if(key.updated_at != '') {
						me.searchStr += '；';
					}
				}
				key.status = this.tabIndex;
				var me = this;
				this.$get("admin/recommend/findRecommendPosition", key)
					.then2(function(response) {
						for(var i = 0; i < response.data.data.length; i++) {
							response.data.data[i].type = me.types[response.data.data[i].type];
						}
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
<style>
	.input-size{
		width: 87%	
	}
</style>
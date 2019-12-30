<template scoped>
	<div>
		<!--    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="位置总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="首页推荐" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="首页广告" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="内页推荐" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row>
-->
		<!--查询弹出框-->
		<el-dialog title="查询推荐内容" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-input placeholder="请输入产品名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="产品名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center; margin-left: -10px;">
							<el-checkbox label="推荐位置" v-model="search.useRecommendPositionId" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%; margin-left: 10px;" v-model="search.data.recommendPositionId" placeholder="请选择推荐位置" :disabled="!search.useRecommendPositionId">
								<el-option
							      v-for="item in search.position_options"
							      :key="item.id"
							      :label="item.name"
							      :value="item.id">
							    </el-option>
							</el-select>
						</el-col>
					</el-row>
				</el-col>
				<el-col :span="12">
					<el-input placeholder="请输入排序" v-model="search.data.sortNumber" :disabled="!search.useSortNumber" class="bottom20 input-size">
						<el-checkbox label="排序" slot="prepend" v-model="search.useSortNumber"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="5" style="text-align: center; margin-left: -10px;">
							<el-checkbox label="类型" v-model="search.useType" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 100%; margin-left: 10px;" v-model="search.data.type" placeholder="请选择类型" :disabled="!search.useType">
								<el-option label="产品" value="1"></el-option>
								<el-option label="广告" value="4"></el-option>
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
						<el-table-column prop="name" label="产品名称">
						</el-table-column>
						<el-table-column prop="recommendPositionName" label="推荐位置">
						</el-table-column>
						<el-table-column prop="type" label="类型">
						</el-table-column>
						<el-table-column type="recommendPic" label="推荐图片">
							<template slot-scope="props">
								<img v-bind:src="props.row.recommendPic" width="50%" height="50%">
							</template>
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
						<el-form-item label="推荐位置">
							<el-select v-model="form.data.recommendPosition_id" placeholder="请选择" style="width: 100%">
								<el-option v-for="item in RecommendPositions" :key="item.id" :label="item.name" :value="item.id">
								</el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="类型">
							<el-select @change="handleSelectValue" v-model="form.data.type" placeholder="请选择推荐类型" style="width: 100%">
								<el-option label="请选择" value="0"></el-option>
								<el-option label="产品" value="1"></el-option>
								<el-option label="广告" value="4"></el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="产品信息">
							<el-select v-model="form.data.relate_Id" style="width: 100%" placeholder="请选择产品信息">
								<el-option v-for="item in selectValue" :key="item.id" :label="item.name" :value="item.id">
								</el-option>
							</el-select>
						</el-form-item>
						<el-form-item label="推荐图片">
							<el-upload ref="upload" class="upload-demo" v-bind:action="uploadUrl" :on-preview="handlePreview" :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="1" :on-exceed="handleExceed" :file-list="fileList" :on-success="handleAvatarSuccess" :name="file">
								<el-button size="small" type="primary">点击上传</el-button>
								<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
							</el-upload>
							<img :src="show_image" width="80%" height="80%">
						</el-form-item>
						<el-form-item label="排序">
							<el-input v-model="form.data.sortNumber"></el-input>
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
				show_image: this.baseUrl,
				fileList: [],
				table: {
					total: 11,
					pageCount: 25,
					pageIndex: 1,
					items: [{
						id: '',
						name: '',
						recommendPositionName: '',
						type: '',
						recommendPic: '',
						sortNumber: '',
						updated_at: ''
					}, ],
					selections: [],
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						id: '',
						recommendPic: '',
						recommendPosition_id: '',
						relate_Id: '',
						sortNumber: '',
						type: '',
						path: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					useType: true,
					useSortNumber: true,
					useRecommendPositionId: true,
					data: {
						name: '',
						recommendPositionId: '',
						type: '',
						sortNumber: '',
					},
					position_options: [],
				},
				allAdvs: [{
					created_at: '',
					id: '',
					name: '',
					note: '',
					sort_number: '',
					updated_at: '',
					url: ''
				}],
				RecommendPositions: [{
					created_at: '',
					id: '',
					image: '',
					mode: '',
					name: '',
					note: '',
					sortNumber: '',
					type: '',
					updated_at: ''
				}],
				selectValue: [],
				products: [{
					category_id: '',
					created_at: '',
					disable: '',
					disableNote: '',
					id: '',
					name: '',
					isDelete: '',
					isSupportReturn: '',
					is_sale: '',
					logistics_template_id: '',
					mainPic: '',
					minAllowableUnitDeliveryCharge: '',
					minAllowableUnitPrice: '',
					model: '',
					note: '',
					numberReviews: '',
					originUnitPrice: '',
					pricingUnit: '',
					prod_prop: '',
					productId: '',
					ratings: '',
					reviewNum1: '',
					reviewNum2: '',
					reviewNum3: '',
					saleNum: '',
					shop_id: '',
					status: '',
					storeAmount: '',
					suggestedRetailUnitDeliveryCharge: '',
					suggestedRetailUnitPrice: '',
					summary: '',
					supplier_id: '',
					taxId: '',
					unitCost: '',
					unitDeliverCost: '',
					upc: '',
					updated_at: '',
					volume: '',
					weight: ''
				}]
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
			handleAvatarSuccess(val) {
				this.form.data.path = val.path;
				this.form.data.recommendPic = val.path;
				this.show_image = this.baseUrl + val.path;
				console.log('上传成功'+this.show_image);
			},
			handleCreate() {
				this.form.title = "添加记录";
				this.form.data = {};
				this.show_image = '';
				this.form.visible = true;
			},
			handleEdit(id, row) {
				var me = this;
				me.form.visible = true;
				this.$post('admin/recommend/getRecommend', {
						id: row.id
					}).then2(function(response) {
						me.form.data = response.data.recommend;
						me.show_image = me.$getAbsolutePath(me.form.data.path);
						me.handleSelectValue(me.form.data.type);
						me.form.data.type += "";
					}),
					function(response) {
						me.$notify.error({
							title: '错误',
							message: '查看失败',
						});
					};
			},
			handleSelectValue(val) {
				if(val == 1) {
					this.selectValue = this.products;
				} else if(val == 4) {
					this.selectValue = this.allAdvs;
				}
			},
			handleDelete(id, row) {
				var me = this;
				me.$confirm('此操作将删除相关数据, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$post('admin/recommend/deleteRecommend', {
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
					me.$post('admin/recommend/deleteRecommend', {
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
				
				if(data.id) {
					this.$post('admin/recommend/updateRecommend', data).then2(function(response) {
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
					this.$post('admin/recommend/createRecommend', data).then2(function(response) {
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
					me.searchStr += ' 产品名称： ' + key.name ;
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useRecommendPositionId && this.search.data.recommendPositionId) {
					key.recommendPositionId = this.search.data.recommendPositionId;
					var temp;
					switch(parseInt(key.recommendPositionId)){
						case 1:
							temp ="首页轮播图";
							break;
						case 2:
							temp ="自营产品";
							break;
						case 3:
							temp = "文章列表页轮播图";
							break;
						case 7:
							temp ="活动轮播图";
							break;
						case 8:
							temp ="限时特价";
							break;
						case 9:
							temp = "每日热销";
							break;
						case 11:
							temp ="欢度周末，畅游周边";
							break;
						case 12:
							temp ="主题 热门目的地";
							break;
						case 13:
							temp ="每日最新";
							break;
						case 14:
							temp ="地方特产";
							break;
						case 15:
							temp ="地标特产";
							break;
						case 16:
							temp = "周边游";
							break;
						case 17:
							temp ="注册有礼";
							break;
						default:
							temp = key.recommendPositionId;
					}
					me.searchStr += ' 推荐位置：'+key.recommendPositionId +temp;
					if(key.recommendPositionId != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useType && this.search.data.type) {
					key.type = this.search.data.type;
					me.searchStr += ' 类型： ' + (key.type == 1?"产品":"广告");
					if(key.type != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useSortNumber && this.search.data.sortNumber) {
					key.sortNumber = this.search.data.sortNumber;
					me.searchStr += ' 排序： ' + key.sortNumber;
					if(key.sortNumber != '') {
						me.searchStr += '；';
					}
				}
				
				this.$get("admin/recommend/findRecommend", key)
					.then2(function(response) {
						var data = response.data.data;
						for (var i = 0; i < data.length; i++) {
							data[i].path = me.$getAbsolutePath(data[i].path);
							data[i].recommendPic = me.$getAbsolutePath(data[i].recommendPic);
							console.log(data[i].recommendPic);
							data[i].type = me.$getRecommendType(data[i].type);
						}
						me.table.items = data;
						me.table.total = response.data.totalRow;
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
			},
			getAllAdvs() {
				var me = this;
				this.$post('admin/recommend/getAllAdvs')
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							me.allAdvs = response.data.data;
						}
					}).catch(function(err) {
						console.log(err)
					});
			},
			recommendPositions() {
				var me = this;
				this.$post('admin/recommend/RecommendPositions')
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							me.RecommendPositions = response.data.data;
							me.search.position_options = response.data.data;
						}
					}).catch(function(err) {
						console.log(err)
					});
			},
			getproducts() {
				var me = this;
				this.$post('admin/recommend/products')
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							me.products = response.data.data;
						}
					}).catch(function(err) {
						console.log(err)
					});
			},
			handleRemove(file, fileList) {
				console.log(file, fileList);
			},
			handlePreview(file) {
				console.log('aa='+file);
			},
			handleExceed(files, fileList) {
				this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
			},
			beforeRemove(file, fileList) {
				return this.$confirm(`确定移除 ${ file.name }？`);
			}
		},
		mounted: function() {
			this.handleSearch();
			this.recommendPositions();
			this.getAllAdvs();
			this.getproducts();
		}
	}
</script>
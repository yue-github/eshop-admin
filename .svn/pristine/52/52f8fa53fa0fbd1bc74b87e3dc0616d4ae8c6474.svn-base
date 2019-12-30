<template scoped>
	<div>
		<!--查询弹出框-->
	    <el-dialog title="查询产品信息" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
			    <el-col :span="12">
			    	
					<el-input placeholder="请输入产品成本大于" v-model="search.data.unitCostMoreThan" :disabled="!search.useUnitCostMoreThan" class="bottom20 input-size">
						<el-checkbox label="产品成本大于" slot="prepend" v-model="search.useUnitCostMoreThan"></el-checkbox>
					</el-input>
					
					<el-input placeholder="请输入产品成本小于" v-model="search.data.unitCostLessThan" :disabled="!search.useUnitCostLessThan" class="bottom20 input-size">
						<el-checkbox label="产品成本小于" slot="prepend" v-model="search.useUnitCostLessThan"></el-checkbox>
					</el-input>
					
					<el-input placeholder="请输入指导价格大于" v-model="search.data.suggestedRetailUnitPriceMoreThan" :disabled="!search.useSuggestedRetailUnitPriceMoreThan" class="bottom20 input-size">
						<el-checkbox label="指导价格大于" slot="prepend" v-model="search.useSuggestedRetailUnitPriceMoreThan"></el-checkbox>
					</el-input>
					
					<el-input placeholder="请输入指导价格小于" v-model="search.data.suggestedRetailUnitPriceLessThan" :disabled="!search.useSuggestedRetailUnitPriceLessThan" class="bottom20 input-size">
						<el-checkbox label="指导价格小于" slot="prepend" v-model="search.useSuggestedRetailUnitPriceLessThan"></el-checkbox>
					</el-input>
				
		      </el-col>
			   	<el-col :span="12">
				    	
		            <el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20 input-size">
						<el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					
					<el-input placeholder="请输入产品分类" v-model="search.data.categoryName" :disabled="!search.useCategoryName" class="bottom20 input-size">
						<el-checkbox label="产品分类" slot="prepend" v-model="search.useCategoryName"></el-checkbox>
					</el-input>
					
					<el-row class="bottom20">
						<el-col :span="6" style="text-align: center">
							<el-checkbox label="是否已上架" v-model="search.useIs_sale" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select style="width: 93%;" v-model="search.data.is_sale" placeholder="请选择是否已上架" :disabled="!search.useIs_sale">
								<el-option label="是" value="1"></el-option>
								<el-option label="否" value="0"></el-option>
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
		<!-- <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="人员总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row> -->
		<el-row :gutter="20">
			<!--<el-col :span="4">
				<bc-panel title="查询产品信息" icon="el-icon-search">
					<div slot="footer" style="text-align: right">
						<el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
					</div>
					<el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
						<el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入产品分类" v-model="search.data.categoryName" :disabled="!search.useCategoryName" class="bottom20">
						<el-checkbox label="产品分类" slot="prepend" v-model="search.useCategoryName"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入产品成本大于" v-model="search.data.unitCostMoreThan" :disabled="!search.useUnitCostMoreThan" class="bottom20">
						<el-checkbox label="产品成本大于" slot="prepend" v-model="search.useUnitCostMoreThan"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入产品成本小于" v-model="search.data.unitCostLessThan" :disabled="!search.useUnitCostLessThan" class="bottom20">
						<el-checkbox label="产品成本小于" slot="prepend" v-model="search.useUnitCostLessThan"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入指导价格大于" v-model="search.data.suggestedRetailUnitPriceMoreThan" :disabled="!search.useSuggestedRetailUnitPriceMoreThan" class="bottom20">
						<el-checkbox label="指导价格大于" slot="prepend" v-model="search.useSuggestedRetailUnitPriceMoreThan"></el-checkbox>
					</el-input>
					<el-input placeholder="请输入指导价格小于" v-model="search.data.suggestedRetailUnitPriceLessThan" :disabled="!search.useSuggestedRetailUnitPriceLessThan" class="bottom20">
						<el-checkbox label="指导价格小于" slot="prepend" v-model="search.useSuggestedRetailUnitPriceLessThan"></el-checkbox>
					</el-input>
					<el-row class="bottom20">
						<el-col :span="9" style="text-align: center">
							<el-checkbox label="是否已上架" v-model="search.useIs_sale" style="margin-top: 5px"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-select v-model="search.data.is_sale" placeholder="请选择是否已上架" :disabled="!search.useIs_sale">
								<el-option label="是" value="1"></el-option>
								<el-option label="否" value="0"></el-option>
							</el-select>
						</el-col>
					</el-row>
				</bc-panel>
			</el-col>-->
			<el-col :span="4">
				<bc-panel title="产品分类" icon="icon-grid icons">
					<el-tree :props="props1" @node-click="handleNodeClick" :load="loadNode1" lazy>
					</el-tree>
				</bc-panel>
			</el-col>
			<el-col :span="20">
				<bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
					<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
					<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
					<el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="expand" label="图片">
							<template slot-scope="props">
								<el-form label-position="left" inline class="demo-table-expand">
									<el-form-item label="产品图片:">
										<img v-if="props.row.mainPic" v-bind:src="props.row.mainPic" class="image">
									</el-form-item>
								</el-form>
							</template>
						</el-table-column>
						<el-table-column prop="name" label="产品名称">
						</el-table-column>
						<el-table-column prop="categoryName" label="产品分类">
						</el-table-column>
						<el-table-column prop="unitCost" label="产品成本" width="150">
						</el-table-column>
						<el-table-column prop="suggestedRetailUnitPrice" label="指导价格" width="150">
						</el-table-column>
						<el-table-column prop="is_sale" label="是否出售中" width="150">
						</el-table-column>
						<el-table-column label="操作" width="150">
							<template slot-scope="scope">
								<el-button size="small" @click="handleEdit(scope.$index, scope.row)">查看</el-button>
								<!--<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button>-->
								<!--        <el-button size="small">禁用产品</el-button>
                <el-button size="small">查看</el-button> -->
							</template>
						</el-table-column>
					</el-table>
					<el-row slot="footer">
						<!--<el-col :span="8">
							<el-button-group>
								<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
									<i class="icon-minus icons"></i> 禁用
								</el-button>
								<el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
								<el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
								<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
							</el-button-group>
						</el-col>-->
						<el-col style="text-align: right">
							<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="table.pageIndex" :page-sizes="[25, 50, 75, 100]" :page-size="25" layout="total, sizes, prev, pager, next, jumper" :total="table.total">
							</el-pagination>
						</el-col>
					</el-row>
				</bc-panel>

				<el-dialog :title="form.title" v-model="form.visible" size="tiny">
					<el-form ref="form" :model="form.data" label-width="80px">
						<el-form-item label="产品名称">
							<el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="产品分类">
							<el-input v-model="form.data.categoryName" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="产品成本">
							<el-input v-model="form.data.unitCost" :disabled="form.disabled"></el-input>
						</el-form-item>
						<el-form-item label="指导价格">
							<el-input v-model="form.data.suggestedRetailUnitPrice" ></el-input>
						</el-form-item>
						<el-form-item label="是否上架">
							<el-select v-model="form.data.is_sale" style="width: 100%">
								<el-option label="是" value="1"></el-option>
								<el-option label="否" value="0"></el-option>
							</el-select>
						</el-form-item>
						</el-form-item>
						<el-form-item>
							<!--<el-button type="primary" @click="handleSubmit()">立即更新</el-button>-->
							<el-button @click="form.visible=false">取消</el-button>
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
						mainPic: 'haha',
						name: 'jjjj',
						categoryName: 'yyyyy',
						unitCost: '5000',
						suggestedRetailUnitPrice: '50000',
						is_sale: '1'
					}, ],
					selections: [],
				},
				props1: {
					label: 'name',
					children: 'zones',
					isLeaf: 'leaf'
				},
				form: {
					visible: false,
					title: '',
					disabled: false,
					data: {
						name: '',
						sex: 'unkown',
						role: 'query',
						is_sale: '',
						mobilePhone: '',
						address: ''
					}
				},
				search: {
					visible: false,
					useName: true,
					useCategoryName: true,
					useUnitCostMoreThan: true,
					useUnitCostLessThan: true,
					useSuggestedRetailUnitPriceMoreThan: true,
					useSuggestedRetailUnitPriceLessThan: true,
					useIs_sale: true,
					data: {
						name: '',
						categoryName: '',
						unitCostMoreThan: '',
						UnitCostLessThan: '',
						suggestedRetailUnitPriceMoreThan: '',
						suggestedRetailUnitPriceLessThan: '',
						is_sale: '',
						category_id:''
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
				this.form.title = "产品详情";
				this.form.disabled = false;
				this.form.data.name = '';
				this.form.data.sex = 'unkown';
				this.form.data.role = 'query';
				this.form.data.mobilePhone = '';
				this.form.data.address = '';
				this.form.visible = true;
			},

			handleNodeClick(data, node, vuecomponen) {
				var me = this;
				/*this.$get('admin/property/getChildCategories',{
					parent: node.data.id
				})
					.then2(function(response) {
						if(response.error > 0) {
							alert(response.errmsg);
						} else {
							me.table.items = response.data.data;
						}
					}).catch(function(err) {
						console.log(err)
					});*/
				this.search.data.category_id = node.data.id;
				this.handleSearch();
			},
			loadNode1(node, resolve) {
				var me = this;
				console.log(node);

				if(node.level === 0) {
					return resolve([{
						name: '所有',
						id: 0
					}]);
				}

				if(node.level === 1) {
					this.$get('admin/property/getChildCategories', {
							parent: 0
						}).then2(function(response) {
							//me.table.items = response.data.data;
							return resolve(response.data.data);
						}),
						function(response) {
							me.$notify.error({
								title: '错误',
								message: '查看失败',
							});
						};
					return;
				}

				if(node.level > 1) {
					parent = node.data.id;
					this.$get('admin/property/getChildCategories', {
							parent: node.data.id
						}).then2(function(response) {
							//me.table.items = response.data.data;
							return resolve(response.data.data);
						}),
						function(response) {
							me.$notify.error({
								title: '错误',
								message: '查看失败',
							});
						};
					return;
				}
			},

			handleEdit(id, row) {
				this.form.title = "产品详情";
				this.form.disabled = true;
				this.form.data.name = row.name;
				this.form.data.unitCost = row.unitCost;
				this.form.data.categoryName = row.categoryName;
				if(row.is_sale == "是"){
					this.form.data.is_sale = '1';
				}else{
					this.form.data.is_sale = '0';
				}
				this.form.data.suggestedRetailUnitPrice = row.suggestedRetailUnitPrice;
				this.form.visible = true;
			},
			handleDelete(id, row) {

			},
			handleDeleteSelections() {

			},
			handleSubmit() {

			},
			handleSearch() {
				//var data=this.search.data;
				var key = {};
				var me = this;
				me.searchStr = '搜索';
				key.length = this.table.pageCount;
				key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
				
				if(this.search.useCategoryName && this.search.data.categoryName) {
					key.categoryName = this.search.data.categoryName;
					me.searchStr += ' 产品分类： ' + key.categoryName ;
					if(key.categoryName != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useName && this.search.data.name) {
					key.name = this.search.data.name;
					me.searchStr += ' 名称： ' + key.name
					if(key.name != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useUnitCostMoreThan && this.search.data.unitCostMoreThan) {
					key.unitCostMoreThan = this.search.data.unitCostMoreThan;
					me.searchStr += ' 产品成本大于： ' + key.unitCostMoreThan ;
					if(key.unitCostMoreThan != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useUnitCostLessThan && this.search.data.unitCostLessThan) {
					key.unitCostLessThan = this.search.data.unitCostLessThan;
					me.searchStr += ' 产品成本小于： ' + key.unitCostLessThan ;
					if(key.unitCostLessThan != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useSuggestedRetailUnitPriceMoreThan && this.search.data.suggestedRetailUnitPriceMoreThan) {
					key.suggestedRetailUnitPriceMoreThan = this.search.data.suggestedRetailUnitPriceMoreThan;
					me.searchStr += ' 指导价格大于： ' + key.suggestedRetailUnitPriceMoreThan;
					if(key.suggestedRetailUnitPriceMoreThan != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useSuggestedRetailUnitPriceLessThan && this.search.data.suggestedRetailUnitPriceLessThan) {
					key.suggestedRetailUnitPriceLessThan = this.search.data.suggestedRetailUnitPriceLessThan;
					me.searchStr += ' 指导价格小于： ' + key.suggestedRetailUnitPriceLessThan ;
					if(key.suggestedRetailUnitPriceLessThan != '') {
						me.searchStr += '；';
					}
				}
				if(this.search.useIs_sale && this.search.data.is_sale) {
					key.is_sale = this.search.data.is_sale;
					me.searchStr += '	是否已上架：' + (key.is_sale == 1?"已上架":"未上架");
				}
				key.category_id = this.search.data.category_id;
				var me = this;
				this.$get('admin/product/many', key)
					.then2(function(response) {
						me.table.items = response.data.data;
						me.table.total = response.data.totalRow;
						for(var i = 0; i < me.table.items.length; i++) {
							if(me.table.items[i].is_sale == 1) {
								me.table.items[i].is_sale = '是';
							} else if(me.table.items[i].is_sale == 0) {
								me.table.items[i].is_sale = '否';
							}
						}
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
	.image {
		width: 300px;
		max-height: 300px;
	}
</style>
<template scoped>
	<div>
		<!--查询弹出框-->
		<el-dialog title="查询" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align:left;">
							<el-checkbox label="开始时间" v-model="search.useStartTime" style="margin-top: 5px; margin-left: 10px; "></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.startTime" type="date" placeholder="选择日期" :disabled="!search.useStartTime"></el-date-picker>
						</el-col>
					</el-row>
				</el-col>

				<el-col :span="12">
					<el-row class="bottom20">
						<el-col :span="5" style="text-align:left;">
							<el-checkbox label="结束时间 " v-model="search.useEndTime" style="margin-top: 5px; margin-left: 10px;"></el-checkbox>
						</el-col>
						<el-col :span="16">
							<el-date-picker v-model="search.data.endTime" type="date" placeholder="选择日期" :disabled="!search.useEndTime"></el-date-picker>
						</el-col>
					</el-row>

				</el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="search.visible = false">取 消</el-button>
				<el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
		</el-dialog>

		<el-row :gutter="20" class="bottom20">
			<el-col :span="4">
				<bc-statbox title="IP数" v-bind:part1="ip" back="#58B7FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="PV" v-bind:part1="pv" back="#20A0FF"></bc-statbox>
			</el-col>
			<el-col :span="4">
				<bc-statbox title="UV" v-bind:part1="uv" back="#1D8CE0"></bc-statbox>
			</el-col>
		</el-row>

		<template scoped>
			<div>
				<el-row :gutter="20" class="bottom20">
					<el-col :span="12" class="bottom20">
						<bc-panel title="访问量统计" icon="icon-grid icons">
							<div id="main1" style="height:400px;width:550px;"></div>
						</bc-panel>
					</el-col>
					<el-col :span="12" class="bottom20">
						<bc-panel title="页面访问统计" icon="icon-grid icons">
							<div id="main2" style="height:400px;width:550px;"></div>
						</bc-panel>
					</el-col>
				</el-row>
			</div>
		</template>

		<el-row :gutter="20">
			<el-button type="primary" @click="search.visible = true" style="float: right;margin-top: -40px;margin-right: 30PX;">查询</el-button>
			<el-col :span="12">
				<bc-panel title="页面访问统计" icon="icon-grid icons">
					<el-table :data="table.pages" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						</el-table-column>
						<el-table-column prop="page" label="页面名称" width="423">
						</el-table-column>
						<el-table-column prop="pageNumbers" label="访问量" width="423">
						</el-table-column>

					</el-table>
				</bc-panel>
			</el-col>
			<el-col :span="12">
				<bc-panel title="来源访问统计" icon="icon-grid icons">
					<el-table :data="table.sources" border @selection-change="handleSelectionChange" style="width: 100%">
						<el-table-column type="selection" width="55">
						</el-table-column>
						</el-table-column>
						<el-table-column prop="source" label="来源" width="423">
						</el-table-column>
						<el-table-column prop="sourceNumbers" label="统计" width="423">
						</el-table-column>
					</el-table>
				</bc-panel>
			</el-col>
		</el-row>
	</div>
</template>

<script>
	// 引入基本模板
	let echarts = require('echarts/lib/echarts')
	// 引入柱状图组件
	require('echarts/lib/chart/bar')
	// 引入提示框和title组件
	require('echarts/lib/component/tooltip')
	require('echarts/lib/component/title')
	require('echarts/lib/chart/line')
	module.exports = {
		data: function() {
			return {
				ip: 0,
				pv: 0,
				uv: 0,
				table: {
					//total: 11,
					// pageCount: 25,
					//pageIndex: 1,
					pages: [],
					sources: [],
					selections: [],
				},
				search: {
					visible: false,
					useStartTime: true,
					useEndTime: true,
					data: {
						startTime: '',
						endTime: '',
					}
				},
				sourceMap: {
					1: 'PC端',
					2: '微信端',
					3: 'androidApp',
					4: '苹果App'
				},
				response: {
					delivery: 10,
					receives: 20,
					shifts: 30
				},
				option: {
					color: ['#3398DB'],
					tooltip: {
						trigger: 'axis',
						axisPointer: { // 坐标轴指示器，坐标轴触发有效
							type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
						}
					},
					grid: {
						left: '3%',
						right: '4%',
						bottom: '3%',
						containLabel: true
					},
					xAxis: [{
						type: 'category',
						data: ['IP', 'PV', 'UV'],
						axisTick: {
							alignWithLabel: true
						}
					}],
					yAxis: [{
						type: 'value'
					}],
					series: [{
						type: 'bar',
						barWidth: '60%',
						data: [10, 52, 200, 334, 390, 330, 220]
					}]
				},
				option3: {
					color: ['#3398DB'],
					tooltip: {
						trigger: 'axis',
						axisPointer: { // 坐标轴指示器，坐标轴触发有效
							type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
						}
					},
					grid: {
						left: '3%',
						right: '4%',
						bottom: '3%',
						containLabel: true
					},
					xAxis: [{
						type: 'category',
						data: ['IP', 'PV', 'UV','tt','tt'],
						axisTick: {
							alignWithLabel: true
						}
					}],
					yAxis: [{
						type: 'value'
					}],
					series: [{
						type: 'bar',
						barWidth: '60%',
						data: [10, 52, 200, 334, 390, 330, 220]
					}]
				},
				deliveries: [],
				times: []
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
				// 图二数据
				var option2data = [];
				var option2data1 = [];
				
				var key = {};
				var me = this;
				/*  key.length=this.table.pageCount ;
				  key.pageIndex= this.table.pageIndex;*/
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.search.data.startTime;
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.search.data.endTime;
				}
				//会员页面访问记录查询
				this.$get("admin/visit/countPageAndSource", key)
					.then2(function(response) {
						//回显访问来源类型
						for(var i = 0; i < response.data.data.sources.length; i++) {
							response.data.data.sources[i].source = me.sourceMap[response.data.data.sources[i].source];

						}
						//图二数据
						for(var i = 0; i < response.data.data.pages.length; i++) {
							//alert(response.data.data.pages[i].page);
							option2data[i] = response.data.data.pages[i].page;
							option2data1[i] = response.data.data.pages[i].pageNumbers;
						}
						
						//图二数据
						me.option3.xAxis[0].data = option2data;
						me.option3.series[0].data = option2data1;
						//alert(me.option3.xAxis.data);
						//alert(me.option3.series.data);
						
						me.handleView();
						
						me.table.pages = response.data.data.pages;
						me.table.sources = response.data.data.sources;
						me.table.total = response.data.data.totalRow;
						me.search.visible = false;
					}).catch(function(err) { 
						console.log(err);
					});
					//666
				this.count();

			},
			count() {
				var data = [];
				var key = {};
				if(this.search.useStartTime && this.search.data.startTime) {
					key.startTime = this.search.data.startTime;
				}
				if(this.search.useEndTime && this.search.data.endTime) {
					key.endTime = this.search.data.endTime;
				}
				var me = this;
				//ip,pv,uv统计
				this.$get("admin/visit/count", key)
					.then2(function(response) {

						me.ip = response.data.ip;
						me.pv = response.data.pv;
						me.uv = response.data.uv;
						data[0] = response.data.ip;
						data[1] = response.data.pv;
						data[2] = response.data.uv;
						me.option.series[0].data = data;

					}).catch(function(err) { 
						console.log(err);
					});

			},
			handleView() {
				var myChart1 = echarts.init(document.getElementById('main1'));
				var myChart2 = echarts.init(document.getElementById('main2'));
				myChart1.setOption(this.option);

				myChart2.setOption(this.option3);
				//alert(this.option2.series[0].data);
			}
		},
		mounted: function() {
			this.handleSearch();
		}
	}
</script>
<template scoped>
	<div>
		<el-row :gutter="20" class="bottom20">
			<el-col :span="8">
				<bc-statbox title="月交易金额" v-bind:part1="month" back="#578ebe"></bc-statbox>
			</el-col>
			<el-col :span="8">
				<bc-statbox title="昨日交易金额" v-bind:part1="yestoday" back="#e04a49"></bc-statbox>
			</el-col>
			<el-col :span="8">
				<bc-statbox title="今日交易金额" v-bind:part1="today" back="#44b6ae"></bc-statbox>
			</el-col>
		</el-row>
		<el-row :gutter="20" class="bottom20">
			<el-col :span="12" class="bottom20">
				<bc-panel title="成交订单数" icon="icon-grid icons">
					<div id="main1" style="height:400px;width:800px;"></div>
				</bc-panel>
			</el-col>
			<el-col :span="12" class="bottom20">
				<bc-panel title="本周交易情况" icon="icon-grid icons">
					<div id="main2" style="height:400px;width:800px;"></div>
				</bc-panel>
			</el-col>
		</el-row>
		<!--<testComponent></testComponent>-->
	</div>
</template>

<script>
	//import testComponent from './WangeditorUtil.vue'
	// 引入基本模板
	let echarts = require('echarts/lib/echarts')
	// 引入柱状图组件
	require('echarts/lib/chart/bar')
	// 引入提示框和title组件
	require('echarts/lib/component/tooltip')
	require('echarts/lib/component/title')
	require('echarts/lib/chart/line')
	export default {
		//components: {testComponent},
		data() {
			return {
				response: {
					delivery: 10,
					receives: 20,
					shifts: 30
				},
				option: {
					tooltip: {
						trigger: 'axis',
						position: function(pt) {
							return [pt[0], '10%'];
						}
					},
					title: {
						left: 'center',
						text: '',
					},
					toolbox: {
						feature: {
							dataZoom: {
								yAxisIndex: 'none'
							},
							restore: {},
							saveAsImage: {}
						}
					},
					xAxis: {
						type: 'category',
						boundaryGap: false,
						data: [],
					
					},
					yAxis: {
						type: 'value',
						boundaryGap: [0, '100%']
					},
					dataZoom: [{
						type: 'inside',
						start: 50,
						end: 100
					}, {
						start: 30,
						end: 100,
						handleIcon: 'M10.7,11.9v-1.3H9.3v1.3c-4.9,0.3-8.8,4.4-8.8,9.4c0,5,3.9,9.1,8.8,9.4v1.3h1.3v-1.3c4.9-0.3,8.8-4.4,8.8-9.4C19.5,16.3,15.6,12.2,10.7,11.9z M13.3,24.4H6.7V23h6.6V24.4z M13.3,19.6H6.7v-1.4h6.6V19.6z',
						handleSize: '80%',
						handleStyle: {
							color: '#fff',
							shadowBlur: 3,
							shadowColor: 'rgba(0, 0, 0, 0.6)',
							shadowOffsetX: 2,
							shadowOffsetY: 2
						}
					}],
					series: [{
						name: '成交量(单)',
						type: 'line',
						smooth: true,
						symbol: 'none',
						sampling: 'average',
						itemStyle: {
							normal: {
								color: 'rgb(87, 142, 190)'
							}
						},
						areaStyle: {
							normal: {
								color: echarts.graphic.LinearGradient(0, 0, 0, 1, [{
									offset: 0,
									color: 'rgb(255, 158, 68)'
								}, {
									offset: 1,
									color: 'rgb(255, 70, 131)'
								}])
							}
						},
						data: []
					}]
				},
				option2: {
					tooltip: {
						trigger: 'axis',
						position: function(pt) {
							return [pt[0], '10%'];
						}
					},
			        // 定义样式和数据
			        backgroundColor: '#FBFBFB',			     
			        calculable: true,	
			        xAxis: {
			         
			            type: 'category',
			            boundaryGap: false,
			            data:[]
			        },
			        yAxis: {
			            type: 'value'
			            
			        },
			        series: [{
			            name: '成交量(元)',
			            type: 'line',
			            symbol: 'none',
			            smooth: 0,
			            color: ['#66AEDE'],
			            data: []
			        }]
			    },

				deliveries: [],
				times: [],
				today: 0,
				yestoday: 0,
				month: 0
			};
		},
		methods: {
			
			getMonthDate(){
				var me = this;
				this.$get("admin/index/getMakeABargain")
				.then2(function(response) {

					me.today = response.data.todayTotal;
					me.yestoday = response.data.yesterdayTotal;
					me.month = response.data.monthTotal;
					//设置时间
					me.option.xAxis.data[0] = response.data.sevenDaysBargain.days.day1;
					me.option.xAxis.data[1] = response.data.sevenDaysBargain.days.day2;
					me.option.xAxis.data[2] = response.data.sevenDaysBargain.days.day3;
					me.option.xAxis.data[3] = response.data.sevenDaysBargain.days.day4;
					me.option.xAxis.data[4] = response.data.sevenDaysBargain.days.day5;
					me.option.xAxis.data[5] = response.data.sevenDaysBargain.days.day6;
					me.option.xAxis.data[6] = response.data.sevenDaysBargain.days.day7;
					//设置成交量数
					me.option.series[0].data[0] = response.data.sevenDaysBargain.daysTotal.day1;
					me.option.series[0].data[1] = response.data.sevenDaysBargain.daysTotal.day2;
					me.option.series[0].data[2] = response.data.sevenDaysBargain.daysTotal.day3;
					me.option.series[0].data[3] = response.data.sevenDaysBargain.daysTotal.day4;
					me.option.series[0].data[4] = response.data.sevenDaysBargain.daysTotal.day5;
					me.option.series[0].data[5] = response.data.sevenDaysBargain.daysTotal.day6;
					me.option.series[0].data[6] = response.data.sevenDaysBargain.daysTotal.day7;
					//设置时间
					me.option2.xAxis.data[0] = response.data.sevenDaysMoney.days.day1;
					me.option2.xAxis.data[1] = response.data.sevenDaysMoney.days.day2;
					me.option2.xAxis.data[2] = response.data.sevenDaysMoney.days.day3;
					me.option2.xAxis.data[3] = response.data.sevenDaysMoney.days.day4;
					me.option2.xAxis.data[4] = response.data.sevenDaysMoney.days.day5;
					me.option2.xAxis.data[5] = response.data.sevenDaysMoney.days.day6;
					me.option2.xAxis.data[6] = response.data.sevenDaysMoney.days.day7;
					//设置成交量数
					me.option2.series[0].data[0] = response.data.sevenDaysMoney.daysTotal.day1;
					me.option2.series[0].data[1] = response.data.sevenDaysMoney.daysTotal.day2;
					me.option2.series[0].data[2] = response.data.sevenDaysMoney.daysTotal.day3;
					me.option2.series[0].data[3] = response.data.sevenDaysMoney.daysTotal.day4;
					me.option2.series[0].data[4] = response.data.sevenDaysMoney.daysTotal.day5;
					me.option2.series[0].data[5] = response.data.sevenDaysMoney.daysTotal.day6;
					me.option2.series[0].data[6] = response.data.sevenDaysMoney.daysTotal.day7;
					me.view();
				}).catch(function(err) {
					console.log(err);
				});
			},
			view(){
				var myChart1 = echarts.init(document.getElementById('main1'));
				var myChart2 = echarts.init(document.getElementById('main2'));
				myChart1.setOption(this.option);
				myChart2.setOption(this.option2);
			}
		},
		created() {
			
		},
		mounted: function() {
			this.getMonthDate();
		},
	}
</script>
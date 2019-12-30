<template>
	<div id="app">
		<el-row>
			<el-col :span="20" style="margin-top: 10px;">
				<img src="./assets/home_logo.png" style="width: 170px;height: 50px;" />
				<span class="sm-title">乐驿电子商务平台</span>
			</el-col>
			<el-col :span="4" class="right-text">
				<span class="sm-title">欢迎{{ userName }}</span>
				&nbsp;&nbsp;&nbsp;&nbsp;
				<el-button type="" size="small" @click="layout()">
					<i class="icons"></i>
					登出
				</el-button>
			</el-col>
		</el-row>
		<el-row class="bottom20">
			<el-menu theme="dark" :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
				<div v-for="item in navs">
					<el-menu-item index="0" v-if="!item.children">
						<i v-bind:class="item.icon"></i>
						<router-link v-bind:to="item.url">{{ item.displayName }}</router-link>
					</el-menu-item>
					<el-submenu v-bind:index="String(item.id)" v-if="item.children">
						<template slot="title">
							{{ item.displayName }}
						</template>
						<el-menu-item v-bind:index="String(chil.id)" v-for="(chil, index) in item.children" :key="index">
							<router-link v-bind:to="chil.url">{{ chil.displayName }}</router-link>
						</el-menu-item>
					</el-submenu>
				</div>
			</el-menu>
		</el-row>
		<el-row class="bottom20">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item v-for="(path, index) in keyPath" :key="index">{{ path }}</el-breadcrumb-item>
			</el-breadcrumb>
		</el-row>

		<router-view></router-view>

		<div style="margin-top: 20px">
			<el-row style="height: 40px; line-height: 40px; color: #bbb">
				<el-col :span="20"><small>粤ICP备05071255号-2 | Copyright&copy;2016广东通驿高速公路服务区有限公司</small></el-col>
				<el-col :span="4" style="text-align: right">
					<small>
						更多信息请
						<a href="http://erp.bayou-tech.cn" style="text-decoration: none; color: #bbb">戳此处</a>
					</small>
				</el-col>
			</el-row>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			activeIndex: '0',
			keyPath: ['我的工作台'],
			labels: [],
			userName: '',
			navs: []
		};
	},
	methods: {
		handleSelect(key, keys) {
			this.keyPath = [];
			for (var i = 0; i < keys.length; i++) {
				this.keyPath[i] = this.labels[keys[i]];
			}
		},
		getRoleNavs() {
			var me = this;
			this.$get('admin/index/getRoleNavs')
				.then2(function(response) {
					if (response.data.error > 0) {
						window.location.href = '#/login';
						location.reload();
					}

					var attr = ['我的工作台'];
					var id = 1;
					for (var i = 0; i < response.data.data.length; i++) {
						var children = response.data.data[i].children;

						if (i == 0) {
							attr.push(response.data.data[i].displayName);
						}
						if (children) {
							id++;
							response.data.data[i].id = id;
							attr.push(response.data.data[i].displayName);
							for (var j = 0; j < children.length; j++) {
								attr.push(children[j].displayName);
								id++;
								response.data.data[i].children[j].id = id;
							}
						}
					}
					let obj = {
						created_at:"2018-04-26 15:52:58",
						displayName:" 会员消费行为分析",
						editable:1,
						icon:"",
						id:68,
						name:"GradeRule",
						parent_id:62,
						readable:1,
						rid:1,
						sortNumber:5,
						updated_at:"2018-04-02 16:05:25",
						url:"/mvp_analyze",
					}
					
					me.navs = response.data.data;
					// me.navs[13].children.push(obj)
					me.labels = attr;
				})
				.catch(function(err) {
					//     window.location.href = '#/login';
					//     location.reload();
				});
		},
		layout: function() {
			this.$post('admin/login/logout')
				.then2(function(response) {
					localStorage.token = '';
					window.location.href = '#/login';
					location.reload();
				})
				.catch(function(err) {
					window.location.href = '#/login';
					location.reload();
				});
		}
	},
	mounted: function() {
		this.getRoleNavs();
		this.userName = localStorage.userName;
	}
};
</script>

<style>
body {
	background: #f5f5f5;
}
.bottom20 {
	margin-bottom: 20px;
}
a {
	text-decoration: none;
}
#app {
	width: 100%;
	margin: 0 auto;
	font-family: 'Microsoft YaHei';
}
img {
	vertical-align: middle;
}
.right-text {
	text-align: right;
	line-height: 65px;
}
.el-menu-item a {
	display: inline-block;
	height: 100%;
	width: 100%;
}
.sm-title {
	color: #324057;
}
</style>

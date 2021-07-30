<template>
	<el-container class="home_container">
		<!-- 头部区域 -->
		<el-header>
			<div>
				<img src="static/long.jpg" alt="" />
				<span>电商后台管理系统</span>
			</div>
			<el-button type="info" @click="logout">退出</el-button>
		</el-header>
		<!-- 页面主体区域 -->
		<el-container>
			<!-- 左边侧边栏 -->
			<el-aside :width="isCollapse ? '41px':'200px'">
				<div class="toggle-button" @click="toggleCollapse">|||</div>
				<!-- 侧边栏菜单区域 -->
				<el-col :span="12">
					<el-menu default-active="2" class="el-menu-vertical-demo" @open="handleOpen"
						@close="handleClose" text-color="black" 
						router>
						<el-submenu index="1">
							<template slot="title">
								<i class="el-icon-custom"></i>
								<span>数据统计</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="/view/welcome">数据视图</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='2'>
							<template slot="title">
								<i class="el-icon-menu"></i>
								<span>商品管理</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="goods/list">商品列表</el-menu-item>
								<el-menu-item index="goods/cate">商品分类</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='3'>
							<template slot="title">
								<i class="el-icon-document"></i>
								<span>订单管理</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="order/order">订单列表</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='4'>
							<template slot="title">
								<i class="el-icon-setting"></i>
								<span>会员管理</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="member/mlist">会员列表</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='5'>
							<template slot="title">
								<i class="el-icon-setting"></i>
								<span>库存管理</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="inventory/count">库存盘点</el-menu-item>
								<el-menu-item index="inventory/log">出入库日志</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='6'>
							<template slot="title">
								<i class="el-icon-setting"></i>
								<span>促销管理</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="promotion/discount">商品折扣</el-menu-item>
								<el-menu-item index="promotion/coupons">优惠券</el-menu-item>
								<el-menu-item index="promotion/pmessage">推送消息</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='7'>
							<template slot="title">
								<i class="el-icon-setting"></i>
								<span>店铺设置</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="set/baseinf">店铺信息设置</el-menu-item>
								<el-menu-item index="set/gpicture">广告设置</el-menu-item>
								<el-menu-item index="set/tpicture">商品展示设置</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
					</el-menu>
				</el-col>
			</el-aside>
			<!-- 右侧内容主体 -->
			<el-main>
	<div>
		<!-- 面包屑导航区域 -->
		<el-breadcrumb separator-class="el-icon-arrow-right">
			<el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
			<el-breadcrumb-item>库存管理</el-breadcrumb-item>
			<el-breadcrumb-item>出入库日志</el-breadcrumb-item>
		</el-breadcrumb>
		<!-- 卡片视图区域 -->
		<el-card>
			<!-- 商品table表格区域 -->
			<el-table :data="invlogList" border stripe>
				<el-table-column label="#Id" prop="logId"></el-table-column>
				<el-table-column label="商品名称" prop="gname"></el-table-column>
				<el-table-column label="规格" prop="sname"></el-table-column>
				<el-table-column label="数量" prop="count"></el-table-column>
				<el-table-column label="出/入库" prop="inOrOut"></el-table-column>
				</el-table-column>
			</el-table>
		<!-- 	<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
				:current-page="queryInfo.pagenum" :page-sizes="[5, 10, 15, 20]" :page-size="queryInfo.pagesize"
				layout="total, sizes, prev, pager, next, jumper" :total="total" background>
			</el-pagination> -->
		</el-card>
	</div>
</el-main>
		</el-container>
	</el-container>
</template>

<script>
	export default {
		data() {
			return {
				// 查询参数对象
				// queryInfo: {
				// 	query: '',
				// 	pagenum: 1,
				// 	pagesize: 10
				// },
				// 商品列表
				invlogList: [],
				// 总数据条数
				// total: 0
			}
		},
		created() {
			this.getinvlogList()
		},
		methods: {
			// 获取商品列表
			getinvlogList() {
				uni.request({
					url: 'http://47.98.134.114:8081/log/askAll',
					method: 'GET',
					data: {
						merchantId: 2
					},
					success: res => {
						console.log(res.data, '打印log成功')
						this.invlogList = res.data.data
					}
				})
			},
			handleSizeChange(newSize) {
				this.queryInfo.pagesize = newSize
				this.getinvlogList()
			},
			handleCurrentChange(newPage) {
				this.queryInfo.pagenum = newPage
				this.getinvlogList()
			},
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			},

		}
	}
</script>

<style scoped>
	.home_container {
		height: 100%;
	}
	
	.el-header {
		background-color: #373d41;
		display: flex;
		justify-content: space-between;
		padding-left: 0;
		align-items: center;
		color: #fff;
		font-size: 20px;
	}
	
	.el-header>div {
		display: flex;
		align-items: center;
	}
	
	.el-header>div>span {
		margin-left: 41px;
	}
	
	.toggle-button {
		background-color: #4A5064;
		font-size: 10px;
		line-height: 24px;
		color: rgb(144, 147, 153);
		text-align: center;
		letter-spacing: 0.2em;
		cursor: pointer
	}
	
	.el-menu {
		border-right: none;
	}
	
	.el-main {
		background-color: #eaedf1;
	}
</style>

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
			<el-breadcrumb-item>订单管理</el-breadcrumb-item>
			<el-breadcrumb-item>订单列表</el-breadcrumb-item>
		</el-breadcrumb>
		<!-- 卡片视图区域 -->
		<el-card>
			<el-row>
				<el-col :span="8">
					<el-input placeholder="请输入内容">
						<el-button slot="append" icon="el-icon-search"></el-button>
					</el-input>
				</el-col>
			</el-row>
			<!-- 订单列表数据 -->
			<el-table :data="orderList" border stripe>
				<el-table-column label="订单编号" prop="orderId"></el-table-column>
				<el-table-column label="商品名" prop="gname"></el-table-column>
				<el-table-column label="用户名" prop="userName "></el-table-column>
				<el-table-column label="订单价格" prop="totalPrice"></el-table-column>
				<el-table-column label="订单状态" prop="orderStatus">
				</el-table-column>
				<el-table-column label="操作">
					<template slot-scope="scope">
						<el-button icon="el-icon-edit" @click="showbox(scope.row)" type="primary" size="mini" circle></el-button>
					</template>
				</el-table-column>
			</el-table>
			<!-- 修改用户的对话框 -->
			<el-dialog title="修改用户" :visible.sync="editDialogVisible" width="50%" @close="editDialogClosed">
				<!-- 内容主体区域 -->
				<el-form :model="editForm" ref="editFormRef" label-width="70px">
					<el-form-item label="订单编号">
						<el-input v-model="editForm.orderId" disabled></el-input>
					</el-form-item>
					<el-form-item label="订单状态">
						<el-select v-model="editForm.orderStatus">
							<el-option label="未支付" value="未支付"></el-option>
							<el-option label="待发货" value="待发货"></el-option>
							<el-option label="待收货" value="待收货"></el-option>
							<el-option label="已完成" value="已完成"></el-option>
						</el-select>
					</el-form-item>
				</el-form>
				<!-- 底部区域 -->
				<span slot="footer" class="dialog-footer">
					<el-button @click="editDialogVisible = false">取 消</el-button>
					<el-button type="primary" @click="editorderStatus">确 定</el-button>
				</span>
			</el-dialog>
			<!-- 分页区域 -->
			<!-- <el-pagination
			@current-change="aaaa"
			  :page-size="20"
			  :pager-count="11"
			  layout="prev, pager, next"
			  :total="1000">
			</el-pagination> -->
		</el-card>
		</el-dialog>

	</div>
</el-main>
		</el-container>
	</el-container>
</template>

<script>
	export default {
		data() {
			return {
				// queryInfo: {
				// 	query: '',
				// 	pagenum: 1,
				// 	pagesize: 10
				// },
				total: 0,
				orderList: [],
				orderStatus: [],
				editForm: {
					orderId: '',
					orderStatus: ''
				},
				editDialogVisible: false,
			}
		},
		created() {
			this.getOrderList()
		},
		methods: {
			// aaaa(currentPage ){
				
			// 	console.log(currentPage );
			// },
			getOrderList() {
				uni.request({
					url: 'http://47.98.134.114:8081/order/findOrdersByMerId',
					method: 'GET',
					data: {
						merchantId: 2
					},
					success: res => {
						console.log(res, '打印Res')

						if (res.data.code == 0) {
							this.total = res.data.data.lenth
							this.orderList = res.data.data

						}
					}
				})
			},
			handleSizeChange(newSize) {
				this.queryInfo.pagesize = newSize
				this.getOrderList()
			},
			handleCurrentChange(newPage) {
				this.queryInfo.pagenum = newPage
				this.getOrderList()
			},
			// 展示编辑用户的对话框
			showbox(row) {
				console.log(row);
				this.editForm = {
						orderId: row.orderId,
						orderStatus: row.orderStatus
					}
				this.editDialogVisible = true
			},
			
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			},
			// 监听修改用户对话框的关闭事件
			editDialogClosed() {
				this.$refs.editFormRef.resetFields()
			},
			// 根据编号修改订单状态
			async editorderStatus(id) {
				// cancel confirm
				const result = await this.$confirm('此操作将修改订单状态, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning',
				}).catch(err => err)

				//取消更改
				if (result === 'cancel') {
					return this.$message.info({
						message: '已取消更改',
						duration: 1500,
					})
				}

				uni.request({
					url: 'http://47.98.134.114:8081/order/updateStatus',
					method: 'GET',
					data: {
						orderId: this.editForm.orderId,
						orderStatus: this.editForm.orderStatus
					},
					success: res => {
						console.log(res, '打印Res')

						if (res.data.code == 0) {
							console.log(res.data.msg)
							uni.showToast({
								title: '修改成功！',
								duration: 2000
							})
						}
					}
				});
				// 关闭对话框
				this.editDialogVisible = false
				// 更新数据列表
				this.getOrderList()
				// 提示修改成功
				this.$message.success('修改订单状态成功')
			}
		}
	}
</script>

<style scoped>
	.el-cascader {
		width: 100%;
	}
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

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
			<el-breadcrumb-item :to="{ path: '/view/home' }">首页</el-breadcrumb-item>
			<el-breadcrumb-item>库存管理</el-breadcrumb-item>
			<el-breadcrumb-item>库存盘点</el-breadcrumb-item>
		</el-breadcrumb>
		<!-- 卡片视图区域 -->
		<el-card>
			<!-- 商品table表格区域 -->
			<!-- 展开子表格 -->
			<!-- <el-table-column type="expand" align="center">
				<template slot-scope="props">
					<el-table :data="props.row.dicts">
						<el-table-column prop="label" label="名称">
						</el-table-column>
						<el-table-column prop="value" label="值">
						</el-table-column>
						<el-table-column prop="sort" label="排序">
						</el-table-column>
						<el-table-column prop="remarks" show-overflow-tooltip label="备注">
						</el-table-column>
					</el-table>
				</template>
			</el-table-column> -->
			<!-- 父表格 -->
			<!-- 数据表格 -->
			<!-- <el-table row-key="id"
			        fit
			        highlight-current-row
			        :data="pageData.results"
			        :header-row-class-name="'table-head-th'"
			        @expand-change="handleExpendRow"> -->
			<el-table :data="goodsList" border stripe>
				<el-table-column label="商品Id" prop="sid"></el-table-column>
				<el-table-column label="规格Id" prop="gid"></el-table-column>
				<el-table-column label="规格描述" prop="specification"></el-table-column>
				<el-table-column label="库存量" prop="inventory"></el-table-column>
				<el-table-column label="操作">
					<template slot-scope="scope">
						<el-button type="primary" icon="el-icon-edit" @click="showbox(scope.row)" size="mini">
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<!-- 修改库存的对话框 -->
			<el-dialog title="编辑库存" :visible.sync="editDialogVisible" width="50%" @close="editDialogClosed">
				<!-- 内容主体区域 -->
				<el-form :model="editForm" ref="editFormRef" label-width="70px">
					<el-form-item label="规格Id">
						<el-input v-model="editForm.gid" disabled></el-input>
					</el-form-item>
					<el-form-item label="规格描述">
						<el-input v-model="editForm.specification" disabled></el-input>
					</el-form-item>
					<el-form-item label="库存量">
						<el-input v-model="editForm.inventory"></el-input>
					</el-form-item>
				</el-form>
				<!-- 底部区域 -->
				<span slot="footer" class="dialog-footer">
					<el-button @click="editDialogVisible = false">取 消</el-button>
					<el-button type="primary" @click="editinventory">确 定</el-button>
				</span>
			</el-dialog>
			<!-- <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
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
				goodsList: [],
				// 总数据条数
				// total: 0,
				editForm: {
					gid: '',
					specification: [],
					inventory: ''
				},
				editDialogVisible: false,
			}
		},
		created() {
			this.getGoodsList()
		},
		methods: {
			// 获取商品列表
			getGoodsList() {
				uni.request({
					url: 'http://47.98.134.114:8081/goodsItem/getGoodsList',
					method: 'GET',
					data: {
						merchantId: 2
					},
					success: res => {
						if (res.data.code == 0) {
							console.log(res.data, '打印Res')
							this.goodsList = res.data.data
						}
					}
				})
			},
			handleSizeChange(newSize) {
				this.queryInfo.pagesize = newSize
				this.getGoodsList()
			},
			handleCurrentChange(newPage) {
				this.queryInfo.pagenum = newPage
				this.getGoodsList()
			},
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			},
			// 展示编辑用户的对话框
			showbox(row) {
				console.log(row);
				this.editForm = {
					gid: row.gid,
					specification: row.specification,
					inventory: row.inventory
				}
				this.editDialogVisible = true
			},
			// 监听修改用户对话框的关闭事件
			editDialogClosed() {
				this.$refs.editFormRef.resetFields()
			},
			// 根据规格Id修改库存量
			async editinventory(id) {
				// cancel confirm
				const result = await this.$confirm('此操作将修改库存量, 是否继续?', '提示', {
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
					url: 'http://47.98.134.114:8081/goods/changeInventory',
					method: 'GET',
					data: {
						gid: this.editForm.gid,
						inventory: this.editForm.inventory,
					},
					success: res => {
						console.log(res, '打印Res')

						if (res.data.code == 0) {
							console.log(res.data.data)
							this.$message.success('编辑成功')
							this.getGoodsList()
						} else if (res.data.code == 88) {
							console.log(res.data.msg)
							this.$message.error('规格不存在！')
						}
					}
				})
				// 关闭对话框
				this.editDialogVisible = false
			}

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

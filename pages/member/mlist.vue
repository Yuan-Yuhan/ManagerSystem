<template>
	<el-container class="home_container">
		<!-- 头部区域 -->
		<el-header>
			<div>
				<!-- <img src="static/logo.jpg" alt="" /> -->
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
					<el-menu default-active="2" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose"
						text-color="black" router>
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
						<el-breadcrumb-item>会员管理</el-breadcrumb-item>
						<el-breadcrumb-item>管理会员列表</el-breadcrumb-item>
					</el-breadcrumb>
					<!-- 卡片视图区域 -->
					<el-card>

						<!-- 搜素与添加区域 -->
						
						<!-- 会员列表区域 -->
						<el-table :data="memberList" style="width: 100%" border stripe>
							<el-table-column type="index"> </el-table-column>
							<el-table-column prop="nikename" label="用户名"> </el-table-column>
							<el-table-column prop="gender" label="性别"> </el-table-column>
							<el-table-column prop="birthday" label="出生日期"></el-table-column>
							<el-table-column prop="location" label="所在地"> </el-table-column>
							<el-table-column prop="level" label="会员等级"> </el-table-column>
							<!-- <el-table-column prop="discount" label="会员折扣"> </el-table-column> -->
							<!-- scope.row  能接收到这一行的数据 -->
							<!-- <el-switch v-model="scope.row.mg_state" @change="userStateChanged(scope.row)"> </el-switch> -->
							</el-table-column>
							<el-table-column label="操作" width="180px">
								<template v-slot="scope">
									<!-- 修改按钮 -->
									<el-button size="mini" round type="primary" icon="el-icon-edit"
										@click="showEditDialog(scope.row.id)">
									</el-button>
								</template>
							</el-table-column>
						</el-table>

						<!-- 分页区域 -->
						<!-- 
        size-change 每页显示多少发生变化
        current-change 页码发生变化 
        current-page 当前显示的是第几页
        layout 需要展示的功能组件
      -->
						<!-- <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="queryInfo.pagenum"
	:page-sizes="[5, 10, 15, 20]" :page-size="queryInfo.pagesize" layout="total, sizes, prev, pager, next, jumper"
	:total="total">
</el-pagination> -->

						
						<!-- 修改用户的对话框 -->
						<el-dialog @close="editDialogClose" title="修改用户信息" :visible.sync="editDialogVisible"
							width="40%">
							<el-form :model="editForm" :rules="editFormRules" ref="editFormRef" label-width="70px">
								<el-form-item label="会员等级" prop="level">
									<el-input v-model="editForm.level"></el-input>
								</el-form-item>
							</el-form>
							<span slot="footer" class="dialog-footer">
								<el-button @click="editDialogVisible = false">取 消</el-button>
								<el-button type="primary" @click="editUserInfo()">确 定</el-button>
							</span>
						</el-dialog>
					</el-card>
				</div>
			</el-main>
		</el-container>
	</el-container>
</template>

<script>
	export default {
		name: 'Users',
		data() {
			return {
				// 获取用户数据列表的参数对象
				// queryInfo: {
				// 	query: '', //查询参数
				// 	pagenum: 1, //当前页码
				// 	pagesize: 10, //每页显示多少条数据
				// },
				memberList: [],
				// total: 0,
				// 控制添加用户对话框的显示与隐藏
				addDialogVisible: false,
				// 添加用户的表单数据
				// addForm: {
				// 	nikename: '',
				// 	gender: '',
				// 	birthday: '',
				// 	location: '',
				// 	level: '',
				// },
				// 控制修改用户信息的对话框
				editDialogVisible: false,
				// 查询到的用户信息对象
				editForm: {
					level: ''
				},
				// 已选中的角色ID值
				selectedRoleId: '',
			}
		},
		created() {
			this.getmemberList()
		},
		methods: {
			// 获取会员列表数据
			getmemberList() {
				uni.request({
					url: 'http://47.98.134.114:8081/vip/getAllVIPs',
					method: 'GET',
					data: {
						merchantId: 2
					},
					success: res => {
						console.log(res, '打印Res')

						if (res.data.code == 0) {
							console.log(res.data.data)
						}
						this.memberList = res.data.data
					}
				})
			},
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			},
			// 监听pagesize改变的时间
			handleSizeChange(newSize) {
				this.queryInfo.pagesize = newSize
				this.getmemberList()
			},
			// 监听页码值改变的事件
			handleCurrentChange(newPage) {
				this.queryInfo.pagenum = newPage
				this.getmemberList()
			},
			// 监听添加用户对话框的关闭事件
			addDialogClose() {
				// 重置表单
				this.$refs.addFormRef.resetFields()
			},
			// 显示编辑用户的对话框
			async showEditDialog(id) {
				this.editDialogVisible = !this.editDialogVisible

				const {
					data: res
				} = await this.$http.get('users/' + id)
				if (res.meta.status !== 200) {
					return this.$message.error({
						message: res.meta.msg,
						duration: 1000,
					})
				}
				this.editForm = res.data
			},
			// 编辑用户对话框的关闭事件
			editDialogClose() {
				this.$refs.editFormRef.resetFields()
			},
			// 修改用户信息并提交
			editUserInfo() {
				// this.$refs.editFormRef.validate(async (valid, field) => {
				// 	// 有未校验通过的字段
				// 	if (!valid) {
				// 		let arr = Object.keys(field)
				// 		return this.$message.error({
				// 			message: field[arr[0]][0].message,
				// 			duration: 1500,
				// 		})
				// 	}
				// 	},
				uni.request({
						url: 'http://47.98.134.114:8081/vip/changeLevelAndDiscount',
						method: 'GET',
						data: {
							level: this.level
						},
						success: res => {
							console.log(res, '打印Res')
							if (res.data.code == 0) {
								uni.showToast({
									title: '编辑成功'
								})
								console.log(res.data.msg)
							} else if (res.data.code == 804) {
								console.log(res.data.msg)
								uni.showToast({
									icon: 'none',
									title: '未输入或错误的会员等级！'
								})
							} else if (res.data.code == 803) {
								console.log(res.data.msg)
								uni.showToast({
									icon: 'none',
									title: '错误的VIPID！'
								})
							}
						}
					}),
					// 隐藏对话框
					this.editDialogVisible = !this.editDialogVisible,
					// 更新数据列表
					this.getmemberList()
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

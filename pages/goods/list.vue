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
								<el-menu-item index="list">商品列表</el-menu-item>
								<el-menu-item index="cate">商品分类</el-menu-item>
							</el-menu-item-group>
						</el-submenu>
						<el-submenu index='3'>
							<template slot="title">
								<i class="el-icon-document"></i>
								<span>订单管理</span>
							</template>
							<el-menu-item-group>
								<el-menu-item index="order">订单列表</el-menu-item>
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
			<el-breadcrumb-item>商品管理</el-breadcrumb-item>
			<el-breadcrumb-item>商品列表</el-breadcrumb-item>
		</el-breadcrumb>
		<!-- 卡片视图区域 -->
		<el-card>
			<el-row :gutter="20">
				<el-col :span="4">
					<el-button type="primary" @click="showpbox()">添加商品</el-button>
				</el-col>
			</el-row>
			<!-- 商品table表格区域 -->
			<!-- 展开行 -->
			<el-table-column type="expand">
			  <template slot-scope="props">
			    <!-- {{scope.row}} -->
			    <el-form label-position="left" inline class="demo-table-expand">
			      <el-form-item label="商品ID">
			        <span>{{ props.row.goods_name }}</span>
			      </el-form-item>
			      <el-form-item label="商品名">
			        <span>{{ props.row.goods_price + '.00' }}</span>
			      </el-form-item>
			      <el-form-item label="详情">
			        <span>{{ props.row.goods_number }}</span>
			      </el-form-item>
			      <el-form-item label="类型">
			        <span>{{ props.row.goods_weight }}</span>
			      </el-form-item>
			      <el-form-item label="运费">
			        <!-- <span>{{ props.row.goods_state }}</span> -->
			        <span>已审核</span>
			      </el-form-item>
			      <el-form-item label="最低价">
			        <span>{{ props.row.add_time | dateFormat }}</span>
			      </el-form-item>
			      <el-form-item label="销量">
			        <span>{{ props.row.upd_time | dateFormat }}</span>
			      </el-form-item>
			      <el-form-item label="评价人数">
			        <i style="color:green;font-size:18px;" v-if="props.row.is_promote" class="el-icon-success"></i>
			        <i style="color:red;font-size:18px;" v-else class="el-icon-error"></i>
			      </el-form-item>
			      <el-form-item label="好评率">
			        <span>{{ props.row.hot_mumber }}</span>
			      </el-form-item>
			    </el-form>
			  </template>
			</el-table-column>
			<el-table :data="goodsList" border stripe>
				<el-table-column label="#Id" prop="sid" width="100px"></el-table-column>
				<el-table-column label="商品名称" prop="gname" width="600px"></el-table-column>
				<el-table-column label="类型" prop="type"></el-table-column>
				<el-table-column label="最低价" prop="lowestprice"></el-table-column>
				<el-table-column label="操作">
					<template slot-scope="scope">
						<el-button type="primary" icon="el-icon-edit" size="mini" @click="showbox(scope.row)">
						</el-button>
						<el-button type="danger" icon="el-icon-delete" size="mini" @click="removeById(scope.row)">
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<!-- 修改商品的对话框 -->
			<el-dialog title="编辑商品" :visible.sync="editDialogVisible" width="50%" @close="editDialogClosed">
				<!-- 内容主体区域 -->
				<el-form :model="editForm" ref="editFormRef" :rules="editFormRules" label-width="70px">
					<el-form-item label="商品Id">
						<el-input v-model="editForm.sid" disabled></el-input>
					</el-form-item>
					<el-form-item label="商品名">
						<el-input v-model="editForm.gname"></el-input>
					</el-form-item>
					<el-form-item label="详情">
						<el-input v-model="editForm.details"></el-input>
					</el-form-item>
					<el-form-item label="类型">
						<el-input v-model="editForm.type"></el-input>
					</el-form-item>
					<el-form-item label="运费">
						<el-input v-model="editForm.transportfee"></el-input>
					</el-form-item>
					<el-form-item label="销量">
						<el-input v-model="editForm.sales" disabled></el-input>
					</el-form-item>
					<el-form-item label="评论人数">
						<el-input v-model="editForm.rateCount" disabled></el-input>
					</el-form-item>
					<el-form-item label="好评率">
						<el-input v-model="editForm.favorablerate" disabled></el-input>
					</el-form-item>
				</el-form>
				<!-- 底部区域 -->
				<span slot="footer" class="dialog-footer">
					<el-button @click="editDialogVisible = false">取 消</el-button>
					<el-button @click="showgbox()">继续(添加规格)</el-button>
					<el-button type="primary" @click="editgoodsList">保存并退出</el-button>
				</span>
			</el-dialog>
			<!-- 添加商品的对话框 -->
			<el-dialog title="添加商品" :visible.sync="addgDialogVisible" width="50%" @close="addgDialogClosed">
				<!-- 内容主体区域 -->
				<el-form :model="addgForm" ref="addgFormRef" :rules="addgFormRules" label-width="70px">
					<el-form-item label="商品名">
						<el-input v-model="addgForm.gname"></el-input>
					</el-form-item>
					<el-form-item label="详情">
						<el-input v-model="addgForm.details"></el-input>
					</el-form-item>
					<el-form-item label="类型">
						<el-input v-model="addgForm.type"></el-input>
					</el-form-item>
					<el-form-item label="运费">
						<el-input v-model="addgForm.transportfee"></el-input>
					</el-form-item>
				</el-form>
				<!-- 底部区域 -->
				<span slot="footer" class="dialog-footer">
					<el-button @click="addgDialogVisible = false">取 消</el-button>
					<el-button @click="showgbox()">继续(添加规格)</el-button>
					<el-button type="primary" @click="addggoodsList()">保存并退出</el-button>
				</span>
			</el-dialog>
			<!-- 修改规格的对话框 -->
			<el-dialog title="修改规格" :visible.sync="addsDialogVisible" width="50%" @close="addsDialogClosed">
				<!-- 内容主体区域 -->
				<el-form :model="editsForm" ref="editsFormRef" label-width="70px" class="demo-dynamic">
					<el-form-item label="商品Id">
						<el-input v-model="editForm.sid" disabled></el-input>
					</el-form-item>
					<el-form-item label="商品名">
						<el-input v-model="editForm.gname" disabled></el-input>
					</el-form-item>
					<el-form-item v-for="(specification, index) in editsForm.specification" :label="'规格' + index"
						:key="specification.key" :prop="'specification.' + index + '.value'" :rules="{
					      required: true, message: '请填写规格', trigger: 'blur'
					    }">
						<el-input v-model="specification.value"></el-input>
						<el-button @click.prevent="removeSpecification(specification)">删除</el-button>
					</el-form-item>
					<el-form-item>
						<el-button type="primary" @click="submitForm('editsForm')">提交</el-button>
						<el-button @click="adds('editsForm')">新增规格</el-button>
						<el-button @click="resetForm('editsForm')">重置</el-button>
					</el-form-item>
				</el-form>
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
				queryInfo: {
					query: '',
					pagenum: 1,
					pagesize: 10
				},
				// 商品列表
				goodsList: [],
				// goodsInf:{
				sid: '',
				// 	gname: '',
				// 	type: '',
				// 	lowestprice: ''
				// },
				// 总数据条数
				total: 0,
				editForm: {
					sid: '',
					gname: '',
					details: '',
					type: '',
					transportfee: '',
					sales: '',
					rateCount: '',
					favorablerate: '',
				},
				editsForm: {
					specification: [{
						value: ''
					}],
				},
				addgForm: {
					gname: '',
					details: '',
					type: '',
					transportfee: ''
				},
				editDialogVisible: false,
				addsDialogVisible: false,
				addgDialogVisible: false
			}
		},
		// 这是编辑商品表单验证规则对象
		editFormRules: {
			// 验证商品名是否合法
			gname: [{
				required: true,
				message: '请输入商品名',
				trigger: 'blur'
			}],
			// 验证详情是否合法
			details: [{
				required: true,
				message: '请输入商品详情',
				trigger: 'blur'
			}],
			// 验证类型是否合法
			type: [{
				required: true,
				message: '请输入商品类型',
				trigger: 'blur'
			}],
			// 验证运费是否合法
			transportfee: [{
				required: true,
				message: '请输入运费（元）',
				trigger: 'blur'
			}],
			// 验证最低价是否合法
			lowestprice: [{
				required: true,
				message: '请输入商品最低价',
				trigger: 'blur'
			}]
		},
		// 这是添加商品表单验证规则对象
		addgFormRules: {
			// 验证商品名是否合法
			gname: [{
				required: true,
				message: '请输入商品名',
				trigger: 'blur'
			}],
			// 验证详情是否合法
			details: [{
				required: true,
				message: '请输入商品详情',
				trigger: 'blur'
			}],
			// 验证类型是否合法
			type: [{
				required: true,
				message: '请输入商品类型',
				trigger: 'blur'
			}],
			// 验证运费是否合法
			transportfee: [{
				required: true,
				message: '请输入运费（元）',
				trigger: 'blur'
			}],
			// 验证最低价是否合法
			lowestprice: [{
				required: true,
				message: '请输入商品最低价',
				trigger: 'blur'
			}]
		},
		created() {
			this.getGoodsList()
		},
		methods: {
			// 根据分页获取对应的商品列表
			getGoodsList() {
				uni.request({
					url: 'http://47.98.134.114:8081/goodsItem/findAllGoodsItem',
					method: 'GET',
					data: {
						merchantId: 2
					},
					success: res => {
						console.log(res.data, '打印Res')
						this.total = res.data.data.length
						this.goodsList = res.data.data
						console.log(this.total)
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
			//打开添加商品表单
			showpbox(row) {
				console.log(row);
				this.addgDialogVisible = true
			},
			editgoodsList(){
				this.editDialogVisible = true
			},
			//发送添加商品请求
			addggoodsList(){
				uni.request({
					url: 'http://47.98.134.114:8081/goodsItem/addOne',
					method: 'POST',
					data: {
						merchantId: 2,
						gname: this.addgForm.gname,
						details: this.addgForm.details,
						type: this.addgForm.type,
						transportfee: this.addgForm.transportfee
					},
					success: res => {
						console.log(res, '打印1')
				
						if (res.data.code == 0) {
							this.$message.success('添加成功！')
							this.getGoodsList()
							console.log(res,'打印商家');
						} else if (res.data.code == 5) {
							console.log(res.data.msg)
							this.$message.error('添加失败！')
						}
					}
				})
				this.addgDialogVisible = false
			},
			//删除表单中商品
			async removeById(row) {
				const confirmResult = await this.$confirm('此操作将永久删除该商品, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).catch(error => error)
				if (confirmResult !== 'confirm') {
					return this.$message.info('已取消删除！')
				}
				this.sid = row.sid,
					uni.request({
						url: 'http://47.98.134.114:8081/goodsItem/delete',
						method: 'GET',
						data: {
							sid: this.sid
						},
						success: res => {
							console.log(res, '打印Res')

							if (res.data.code == 0) {
								this.$message.success('删除成功！')
								this.getGoodsList()
							} else if (res.data.code == 99) {
								console.log(res.data.msg)
								this.$message.error('商品不存在！')
							}
						}
					})
			},
			//跳转到修改规格的对话框
			showgbox(row) {
				console.log(row);
				this.editDialogVisible = false,
					this.addgDialogVisible = false,
					this.addsDialogVisible = true
			},
			// 展示编辑用户的对话框
			showbox(row) {
				console.log(row);
				this.editForm = {
					sid: row.sid,
					gname: row.gname,
					details: row.details,
					type: row.type,
					transportfee: row.transportfee,
					lowestprice: row.lowestprice,
					sales: row.sales,
					rateCount: row.rateCount,
					favorablerate: row.favorablerate
				}
				this.editDialogVisible = true
			},
			// 监听修改用户对话框的关闭事件
			editDialogClosed() {
				this.$refs.editFormRef.resetFields()
			},
			//编辑商品
			editgoodsList() {
				uni.request({
					url: 'http://47.98.134.114:8081/goodsItem/update',
					method: 'GET',
					data: {
						gname: this.editForm.gname,
						details: this.editForm.details,
						type: this.editForm.type,
						transportfee: this.editForm.transportfee,
						lowestprice: this.editForm.lowestprice,
						sales: this.editForm.sales,
						rateCount: this.editForm.rateCount,
						favorablerate: this.editForm.favorablerate
					},
					success: res => {
						console.log(res, '打印Res')

						if (res.data.code == 0) {
							this.$message.success('编辑成功！')
							this.getGoodsList()
						} else if (res.data.code == 5) {
							console.log(res.data.msg)
							this.$message.error('商品不存在！')
						}
					}
				})
			},
			//添加规格表单新增规格项
			submitForm(formName) {
				// this.$refs[formName].validate((valid) => {
				//   if (valid) {
				//     alert('submit!');
				//   } else {
				//     console.log('error submit!!');
				//     return false;
				//   }
				// });
				// uni.request({
				// 	url: 'http://47.98.134.114:8081/goods/update',
				// 	method: 'GET',
				// 	data: {
				// 		gname: this.editForm.gname,
				// 		details: this.editForm.details,
				// 		type: this.editForm.type,
				// 		transportfee: this.editForm.transportfee,
				// 		lowestprice: this.editForm.lowestprice,
				// 		sales: this.editForm.sales,
				// 		rateCount: this.editForm.rateCount,
				// 		favorablerate: this.editForm.favorablerate
				// 	},
				// 	success: res => {
				// 		console.log(res, '打印Res')

				// 		if (res.data.code == 0) {
				// 			this.$message.success('编辑成功！')
				// 			this.getGoodsList()
				// 		} else if (res.data.code == 5) {
				// 			console.log(res.data.msg)
				// 			this.$message.error('商品不存在！')
				// 		}
				// 	}
				// })
			},
			resetForm(formName) {
				this.$refs[formName].resetFields();
			},
			removesSecification(item) {
				var index = this.editsForm.specification.indexOf(item)
				if (index !== -1) {
					this.editsForm.specification.splice(index, 1)
				}
			},
			adds() {
				this.editsForm.specification.push({
					value: '',
					key: Date.now()
				});
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

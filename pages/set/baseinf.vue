<template>
	<div>
		<!-- 面包屑导航区域 -->
		<el-breadcrumb separator-class="el-icon-arrow-right">
			<el-breadcrumb-item :to="{ path: '/view/home' }">首页</el-breadcrumb-item>
			<el-breadcrumb-item>店铺设置</el-breadcrumb-item>
			<el-breadcrumb-item>店铺信息设置</el-breadcrumb-item>
		</el-breadcrumb>
		<!-- 卡片视图区域 -->
		<el-card>
			<el-form :data="baseList" :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
				<el-form-item label="电话" prop="merchantPhone">
					<el-input v-model="ruleForm.name"></el-input>
				</el-form-item>
				<el-form-item label="店铺名" prop="merchantName">
					<el-input v-model="ruleForm.name"></el-input>
				</el-form-item>
				<el-form :model="addressForm" :rules="addressFormRules" ref="addressFormRef" label-width="100px">
					<el-form :model="addressForm" :rules="addressFormRules" ref="addressFormRef" label-width="100px">
						<el-form-item label="省市区/县" prop="address1">
							<el-cascader :options="cityData" v-model="addressForm.address1"></el-cascader>
						</el-form-item>
						<el-form-item label="详细地址" prop="address2">
							<el-input v-model="addressForm.address2"></el-input>
						</el-form-item>
						<el-form-item label="密码" prop="password">
							<el-input v-model="ruleForm.password"></el-input>
							</el-form-item>
							<el-form-item label="店铺图" prop="name">
								<el-input v-model="ruleForm.name"></el-input>
								</el-form-item>
					</el-form>
					<el-form-item>
						<el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
						<el-button @click="resetForm('ruleForm')">重置</el-button>
					</el-form-item>
				</el-form>
				</el-form>
		</el-card>
	</div>
</template>

<script>
	// import cityData from './citydata.js'
	// export default {
	// 	data() {
	// 		return {
	// 			ruleForm: {
	// 				phone: '',
	// 				name: '',
	// 				},
	// 				addressForm: {
	// 					address1: [],
	// 					address2: ''
	// 				},
	// 				baseList: [],
	// 				rules: {
	// 					name: [{
	// 							required: true,
	// 							message: '请输入活动名称',
	// 							trigger: 'blur'
	// 						},
	// 						{
	// 							min: 3,
	// 							max: 5,
	// 							message: '长度在 3 到 5 个字符',
	// 							trigger: 'blur'
	// 						}
	// 					]
	// 				},
	// 				addressFormRules: {
	// 					address1: [{
	// 						required: true,
	// 						message: '请选择省市区县',
	// 						trigger: 'blur'
	// 					}],
	// 					address2: [{
	// 						required: true,
	// 						message: '请输入详细地址',
	// 						trigger: 'blur'
	// 					}]
	// 				},
	// 				cityData,,
	// 				progressInfo: [],
	// 				db
	// 			}
	// 			}
	// 		},
	// 		methods: {
	// 			getshopList():{
	// 			uni.request({
	// 			url: 'http://47.98.134.114:8081//merchant/findMerchant', 
	// 			method: 'GET',
	// 			data: {
	// 			    merchantId: 2
	// 			},
	// 			success: res => {
	// 									console.log(res, '打印Res')
				
	// 									if (res.data.code == 0) {
	// 										this.baseList = res.data.data
	// 							})
	// 							},
	// 			submitForm(formName) {
	// 				this.$refs[formName].validate((valid) => {
	// 					if (valid) {
	// 						alert('submit!');
	// 					} else {
	// 						console.log('error submit!!');
	// 						return false;
	// 					}
	// 				});
	// 			},
	// 			resetForm(formName) {
	// 				this.$refs[formName].resetFields();
	// 			}
	// 		}
	// 	}
	// }
</script>

<style scoped>
</style>

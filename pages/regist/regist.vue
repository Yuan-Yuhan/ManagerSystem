<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="static/logo.jpg" alt="" />
      </div>
      <!-- 登录表单区域 -->
      <el-form
        ref="registFormRef"
        :model="registForm"
        :rules="registFormRules"
        label-width="0px"
        class="login_form"
      >
        <!-- 手机号 -->
        <el-form-item prop="phone" placeholder="请输入11位手机号">
          <el-input
            prefix-icon="el-icon-phone"
            v-model="registForm.phone"
          ></el-input>
        </el-form-item>
		<!-- 商户名 -->
		<el-form-item prop="name" placeholder="请输入商户名">
		  <el-input
		    prefix-icon="el-icon-user"
		    v-model="registForm.name"
		  ></el-input>
		</el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password" placeholder="请输入密码">
          <el-input
            prefix-icon="el-icon-lock"
            v-model="registForm.password"
            type="password"
          ></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="regist">注册</el-button>
		  <el-button type="info" @click="$router.push('/pages/view/login')">去登陆</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 这是登录表单的数据绑定对象
      registForm: {
        phone: '',
		name:'',
        password: ''
      },
      // 这是表单验证规则对象
      registFormRules: {
        // 验证用户名是否合法
        phone: [
          { required: true, message: '请输入注册用手机号', trigger: 'blur' },
          { min: 11, max: 11, message: '请输入11位手机号', trigger: 'blur' }
        ],
		// 验证商户名是否合法
		name: [
		  { required: true, message: '请输入商户名称', trigger: 'blur' }
		],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          {
            min: 6,
            max: 15,
            message: '长度在 6 到 15 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm () {
      // console.log(this) // VueComponent
      this.$refs.registFormRef.resetFields()
    },
    regist () {
      this.$refs.registFormRef.validate(async (valid) => {
        // console.log(valid) // false/ture
		      })
		//发起网络请求
    uni.request({
    url: 'http://47.98.134.114:8081//merchant/register', 
	method: 'GET',
    data: {
        phone: this.registForm.phone,
		name: this.registForm.name,
        password: this.registForm.password
    },
    success: res => {
    						console.log(res, '打印Res')
    
    						if (res.data.code == 0) {
								uni.showModal({
								                        title: '注册成功',
								                        content: '点击确定跳转至登陆界面',
								                        success: function (res) {
								                            if (res.confirm) {
								                                console.log('用户点击确定');
																uni.navigateTo({
																	url: '/pages/view/login',
																	 })
								                            } else if (res.cancel) {
								                                console.log('用户点击取消');
								                            }
								                        }
											})
										}
    				// 			uni.showToast({
    				// 				title:'注册成功,即将跳转到登陆界面',
    				// 				duration:2000
    				// 			})
								// setTimeout(function() {
								//          uni.navigateTo({
								//          	url: '/pages/view/login'
								//               })
								//      }, 2000);
								// 	 console.log(res.data.msg)
    				// 			}
    						else if (res.data.code == 3) 
    						{
    							console.log(res.data.msg)
								uni.showToast({
									title:'该号码已被注册！',
									icon:'error',
									duration:2000
								})
    						} 
    						else(res.data.code == 4)
    						{
    							console.log(res.data.msg)
								uni.showToast({
									title:'该商户名已被注册！',
									icon:'error',
									duration:2000
								})
    						}
    
    
    					},
    					fail: () => {
						console.log('fail')
    					},
    					complete: () => {}
    				});
    }
  }
}
</script>

<style scoped>
.login_container {
  background-color: #409eff;
  height: 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.avatar_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}

img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>

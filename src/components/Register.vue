<template>
  <div>
    <el-form
    ref="registerForm"
    :model="registerForm"
    :rules="rules"
    label-width="80px"
    class="login-box"
    >
      <h3 class="login-title">工作报告管理系统</h3>
      <!--账号输入框-->
      <el-form-item label="账号" prop="account">
        <el-input
          type="text"
          placeholder="请输入账号"
          v-model="registerForm.account"
          />
      </el-form-item>
      <!--姓名输入框-->
      <el-form-item label="姓名" prop="name">
         <el-input
                type="text"
                placeholder="请输入姓名"
                v-model="registerForm.name"
         />
      </el-form-item>

      <!--密码输入框-->
      <el-form-item label="密码" prop="password">
        <el-input
          type="password"
          placeholder="请输入密码"
          v-model="registerForm.password"
        />
      </el-form-item>

      <!--密码输入框-->
      <el-form-item label="再次输入" prop="passwordCheck">
        <el-input
         type="password"
         placeholder="请输入密码"
         v-model="registerForm.passwordCheck"
         />
      </el-form-item>



      <el-form-item>
        <!--登录按钮-->
        <el-button type="primary" v-on:click="onSubmit('registerForm')">
          注册
        </el-button>
        <!--注册按钮-->
       <el-button type="primary" v-on:click="upSubmit('')">
          置空
        </el-button>
      </el-form-item>




      <!--提示信息-->
      <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%">
        <span>请输入账号、姓名和密码</span>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="dialogVisible = false">
            确定
          </el-button>
        </span>
      </el-dialog>
    </el-form>
  </div>
</template>



<!--script-->
<script>
  export default{
    name:'Login',
    data(){
      return{
        registerForm:{
          account:'',
          password:'',
          name:''
        },

        rules:{
          account:[{required:true,message:'账号不能为空',trigger:'blur'}],
          password:[{required:true,message:'密码不能为空',trigger:'blur'}],
          name:[{required:true,message:'姓名不能为空',trigger:'blur'}]
        },

        dialogVisible:false
      }
    },

    methods:{
           onSubmit(formName) {
            this.$refs[formName].validate((valid) => {
              if (valid) {
                this.$axios.post("http://localhost:9080/user", this.registerForm)
                 .then(res =>{
                  console.log(res.data)
                  if(res.data.success){
                    this.$router.push('/helloworld')
                  }else{
                    this.$message({
                      message:res.data.message,
                      type:'error'
                    })
                  }
                })
              } else {
                console.log('error submit!!');
                return false;
              }
            });

          },
          resetForm(formName) {
            this.$refs[formName].resetFields();
          }
    }
  }
</script>


<style scoped>
.login-box {
  border: 1px solid #dcdfe6;
  width: 350px;
  margin: 10px auto;
  padding: 35px 35px 15px 35px;
  border-radius: 5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  box-shadow: 0 0 25px #909399;
}

.login-title {
  text-align: center;
  margin: 0 auto 40px auto;
  color: #303133;
}
</style>


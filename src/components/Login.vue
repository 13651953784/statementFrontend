<template>
  <div>
    <el-form
    ref="loginForm"
    :model="form"
    :rules="rules"
    label-width="80px"
    class="login-box"
    >
      <h3 class="login-title">工作报告管理系统</h3>
      <!--账号输入框-->
      <el-form-item label="账号" prop="username">
        <el-input
          type="text"
          placeholder="请输入账号"
          v-model="form.username"
          />
      </el-form-item>

      <!--密码输入框-->
      <el-form-item label="密码" prop="password">
        <el-input
          type="password"
          placeholder="请输入密码"
          v-model="form.password"
        />
      </el-form-item>


      <el-form-item>
        <!--登录按钮-->
        <el-button type="primary" v-on:click="onSubmit('loginForm')">
          登录
        </el-button>
        <!--注册按钮-->
       <el-button type="primary" v-on:click="upSubmit('registerForm')">
          注册
        </el-button>
      </el-form-item>


      <!--提示信息-->
      <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%">
        <span>请输入账号和密码</span>
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

        form:{
          username:'',
          password:''
        },

        rules:{
          username:[{required:true,message:'账号不能为空',trigger:'blur'}],
          password:[{required:true,message:'密码不能为空',trigger:'blur'}]
        },

        dialogVisible:false
      }
    },

    methods:{
      onSubmit(formName){
        this.$refs[formName].validate(
          valid =>{
            if(valid){
              let para = new URLSearchParams()
              para.append('account',this.form.username)
              para.append('password',this.form.password)

              this.$axios
                .get('http://localhost:9080/user',{params:para})

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

                .catch(err =>{
                  console.log(err)
                })
            }else{
              this.dialogVisible = true
              return false
            }
          }
        )
      },


            upSubmit(formName){
              this.$refs[formName].validate(
                valid =>{
                  if(valid){
                    this.$axios
                      .then(res =>{
                          this.$router.push('/register')
                      })
                  }else{
                    this.dialogVisible = true
                    return false
                  }
                }
              )
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

<template>
  <div>
    <el-form
    ref="loginForm"
    :model="form"
    :rules="rules"
    label-width="80px"
    class="login-box"
    >
      <h3 class="login-title">工作报告管理系统</h3>
      <!--账号输入框-->
      <el-form-item label="账号" prop="username">
        <el-input
          type="text"
          placeholder="请输入账号"
          v-model="form.username"
          />
      </el-form-item>

      <!--密码输入框-->
      <el-form-item label="密码" prop="password">
        <el-input
          type="password"
          placeholder="请输入密码"
          v-model="form.password"
        />
      </el-form-item>


      <el-form-item>
        <!--登录按钮-->
        <el-button type="primary" v-on:click="onSubmit('loginForm')">
          登录
        </el-button>
        <!--注册按钮
       <el-button type="primary" v-on:click="upSubmit('registerForm')">
          注册
        </el-button>-->

    <router-link to="/register" tag="button">注册</router-link>


      </el-form-item>


      <!--提示信息-->
      <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%">
        <span>请输入账号和密码</span>
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

        form:{
          username:'',
          password:''
        },

        rules:{
          username:[{required:true,message:'账号不能为空',trigger:'blur'}],
          password:[{required:true,message:'密码不能为空',trigger:'blur'}]
        },

        dialogVisible:false
      }
    },

    methods:{
      onSubmit(formName){
        this.$refs[formName].validate(
          valid =>{
            if(valid){
              let para = new URLSearchParams()
              para.append('account',this.form.username)
              para.append('password',this.form.password)

              this.$axios
                .get('http://localhost:9080/user',{params:para})

                .then(res =>{
                  cons-ole.log(res.data)
                  if(res.data.success){
                    this.$router.push('/helloworld')
                  }else{
                    this.$message({
                      message:res.data.message,
                      type:'error'
                    })
                  }
                })

                .catch(err =>{
                  console.log(err)
                })
            }else{
              this.dialogVisible = true
              return false
            }
          }
        )
      },


            upSubmit(formName){
              this.$refs[formName].validate(
                valid =>{
                  if(valid){
                    this.$axios
                      .then(res =>{
                          this.$router.push('/register')
                      })
                  }else{
                    this.dialogVisible = true
                    return false
                  }
                }
              )
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


<template>
    <div class="login_container">
        <div class='login_box'>
            <!-- 头像区域 -->
            <div class='avater_box'>
                <img src = '../assets/logo.png'>
            </div>
            <el-form :model="loginForm" label-width="0" class="login_form" :rules="loginFormRules"  ref="loginFormRef">
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type='password' v-model="loginForm.password" prefix-icon="iconfont icon-3702mima"></el-input>
                </el-form-item>
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginFrom">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>


<script>
import { log } from 'util';
export default {
    data(){
        return {
            loginForm:{
                username:'admin',
                password:'123456'
            },
            loginFormRules:{
                username:[
                    { required: true, message: '请输入活动名称', trigger: 'blur' },
                    { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true, message: '请输入活动名称', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                ],
            }
        }
    },
    methods:{
        //点击按钮重置登录表单
        resetLoginFrom(){
            this.$refs.loginFormRef.resetFields();
        },
        login(){
            this.$refs.loginFormRef.validate(async valid=>{
                if(!valid) return;
                const { data:res } = await this.$http.post('login',this.loginForm)  
                if(res.meta.status !==200) return this.$message.error('你登录失败了')
                // console.log(res)
                this.$message.success('你登录成功')   
                // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
                //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
                //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
                window.sessionStorage.setItem('token', res.data.token)
                // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
                this.$router.push('/home')                                           
            })
        }
    }
}
</script>

<style lang="less" scoped>
.login_container{
    background-color: #2b4b6b;
    height:100%;

}
.login_box {
    width:450px;
    height:300px;
    background-color:#fff;
    border-radius:3px;
    position:absolute;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);


    .avater_box{
    width:130px;
    height:130px;
    border-radius:50%;
    border:1px solid #eee;
    padding:10px;
    box-shadow:0 0 10px #ddd;
    position:absolute;
    left:50%;
    background-color:#fff;
    transform:translate(-50%,-50%);
        img{
            width:100%;
            height:100%;
            border-radius:50%;
            background:#eee;
        }
    }
}
.login_form{
    position: absolute;
    bottom:0;
    width: 100%;
    padding: 0 20px;
    box-sizing:border-box;

}
.btns{
    display: flex;
    justify-content: flex-end;
}

</style>
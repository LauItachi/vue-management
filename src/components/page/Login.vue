<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">后台管理系统</div>
            <el-form :model="loginForm" ref="loginForm" label-width="0px" class="ms-content">
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" placeholder="username">
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" placeholder="password" v-model="loginForm.password">
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="checkLogin">登录</el-button>
                </div>
                <p class="login-tips">Tips : 用户名和密码随便填。</p>
            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                loginForm: {
                    username: 'admin',
                    password: '12345'
                },
            }
        },
        methods: {
            checkLogin() {
                console.log('checkLogin() called');
                let data = {
                    username: this.loginForm.username,
                    password: this.loginForm.password,
                };
                this.$axios.post("/users/login", data).then( res => { // res: { code, message, result }
                    console.log('POST /users/login Response: '+JSON.stringify(res.data));
                    if( res.data.code === 3301 ){
                        localStorage.setItem('ms_username', res.data.result.username); 
                        this.popNotice('Login successfully', 'Login As '+res.data.result.username);
                        this.$router.push( {name: 'home'} );
                    }
                });
            },
            popNotice(title, content) {
                const h = this.$createElement;
                this.$notify({
                    title: title,
                    message: h('i', { style: 'color: teal'}, content),
                    duration: 2000
                });
            },
        }
    }
</script>

<style scoped>
    .login-wrap{
        position: relative;
        width:100%;
        height:100%;
        background-image: url(../../assets/login-bg.jpg);
        background-size: 100%;
    }
    .ms-title{
        width:100%;
        line-height: 50px;
        text-align: center;
        font-size:20px;
        color: #fff;
        border-bottom: 1px solid #ddd;
    }
    .ms-login{
        position: absolute;
        left:50%;
        top:50%;
        width:350px;
        margin:-190px 0 0 -175px;
        border-radius: 5px;
        background: rgba(255,255,255, 0.3);
        overflow: hidden;
    }
    .ms-content{
        padding: 30px 30px;
    }
    .login-btn{
        text-align: center;
    }
    .login-btn button{
        width:100%;
        height:36px;
        margin-bottom: 10px;
    }
    .login-tips{
        font-size:12px;
        line-height:30px;
        color:#fff;
    }
</style>
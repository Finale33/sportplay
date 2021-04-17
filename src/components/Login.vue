<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="no image" />
            </div>
            <el-form ref="loginFormRef" :rules="loginRules" :model="loginForm" class="login_form" label-width="0">
                <el-form-item prop="username" >
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icondenglu"></el-input>
                </el-form-item>
                <el-form-item prop="password" >
                    <el-input v-model="loginForm.password" prefix-icon="iconfont iconmima" type="password" ></el-input>
                </el-form-item>
                <el-form-item class="btns">
                    <el-button type="primary" @click="login" >提交</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            loginForm: {
                username: "admin",
                password: "123456"
            },
            // 验证对象
            loginRules: {
                username: [
                    {required: true, message: '请输入用户名称', trigger: 'blur'}, 
                    {min: 5, max: 10, message: '长度在3到5个字符', trigger: 'blur'}
                ],
                password: [
                    {required: true, message: '请输入用户密码', trigger: 'blur'}, 
                    {min: 6, max: 10, message: '长度在6到10个字符', trigger: 'blur'}
                ]
            }
        }
    },
    methods: {
        resetLoginForm() {
            this.$refs.loginFormRef.resetFields();
        },
        login() {
            this.$refs.loginFormRef.validate(async valid => {
                if (!valid) return;
                const { data:res } = await this.$http.post("login", this.loginForm);
                console.log(res);
                if(res.flag == 'ok') {
                    window.sessionStorage.setItem("user", res.user);
                    this.$message.success("Login success");
                    this.$router.push({path: "/home"});
                    console.log(res.user);
                } else {
                    this.$message.error("Login failed")
                }
            });
        }
    }
}
</script>

<style lang="less" scoped>
.login_container {
    background-color: #2b4b6b;
    height: 100%;
}
.login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    .avatar_box {
        width: 130px;
        height: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10;
        box-shadow: 0 0 10px #ddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #0ee;
        img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
    }
    .btns {
        display: flex;
        justify-content: flex-end;
    }
    .login_form {
        position: absolute;
        bottom: 0;
        width: 100%;
        padding: 0 10px;
        box-sizing: border-box;
    }
}
</style>

<template>
    <el-container class="home-container">
        <el-header>
            <div>
                <img src="../assets/logo.jpg" >
                <span>个人运动平台</span>
            </div>
            <el-button type="info" @click="logout">安全退出</el-button>
        </el-header>
        <el-container>
            <el-aside :width="isCollapse? '64px' : '200px'">
                <div class="toggle-button" @click="toggleCollapse">|||</div>
                <el-menu 
                    background-color="#545c64" 
                    text-color="#fff" 
                    active-text-color="#409eff" 
                    unique-opened
                    :collapse="isCollapse" 
                    :collapse-transition="false" 
                    :router="true"
                    :default-active="activePath"
                    >
                    <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
                        <template slot="title">
                            <i :class="iconsObject[item.id]"></i>
                            <span>{{item.title}}</span>
                        </template>
                        <el-menu-item :index="it.path" v-for="it in item.slist" :key="it.id" @click="saveNavState(it.path)">
                            <template slot="title">
                            <i :class="iconsObject[it.id]"></i>
                            <span>{{it.title}}</span>
                        </template>
                        </el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <el-main>
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>
<script>
export default {
    data() {
        return {
            menuList: [],
            isCollapse: false,
            iconsObject: {
                '100':'iconfont iconguanliyuan',
                '200':'iconfont iconsport',
                '101':'iconfont icondenglu',
                '102':'iconfont iconmima',
                '103':'iconfont iconsport',
                '104':'iconfont iconshangpin',
                '201':'iconfont iconshu',
                '202':'iconfont iconkaluli',
                '203':'iconfont iconshiwu',
                '204':'iconfont icondenglu',
            },
            activePath: '/welcome',
        }
    },
    created() {
        this.getMenuList();
        this.activePath = window.sessionStorage.getItem('activePath');
    },
    methods: {
        logout() {
            window.sessionStorage.clear();
            this.$router.push("/login");
        },
        async getMenuList() {
            const { data: res } = await this.$http.get("menus");
            console.log(res);
            if (res.status != 200) return this.$message.error("获取列表失败！！！");
            this.menuList = res.data;

        },
        toggleCollapse() {
            this.isCollapse = !this.isCollapse;
        },
        saveNavState(activePath) {
            window.sessionStorage.setItem('activePath', activePath);
            this.activePath = activePath;
        }
    }
}
</script>
<style lang="less" scoped>
.home-container {
    height: 100%;
}

.el-header {
    background-color: #373d41;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-left: 0%;
    color: #fff;
    font-size: 20px;
    >div {
        display: flex;
        align-items: center;
        span {
            margin-left: 15px;
        }
    }
}
.el-aside {
    background-color: #333744;
    .el-menu {
        border-right: none;
    }
}
.el-main {
    background-color: #eaedf1;
}

img {
    width: 55px;
    height: 55px;
}
.toggle-button {
    background-color: #4a5064;
    font-size: 10px;
    line-height: 24px;
    color: #fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
}
</style>
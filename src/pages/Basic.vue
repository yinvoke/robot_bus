<template>
    <div class="page bg-white app-main__container">
        <el-container>
            <!--顶部-->
            <el-header class="top-layout" height="8vh">
                <div class="top-layout-logo" @click="jumphome()">
                    <!--<img src="resource/images/LOGO.png" width="50" height="50">-->
                    <p class="top-layout-title">中间件服务总线控制系统</p>
                </div>
                <div class="top-layout-exit" v-if="username!=null">
                    <el-tooltip class="item"  placement="top-start" effect="light" style="padding:0;margin:0">
                        <div slot="content" class="top-layout-pop">
                            <button @click="jumpUserInfo">我的信息</button><br>
                            <button @click="jumpPassword">修改密码</button><br>
                            <button @click="exitIt">登出账户</button>
                        </div>
                        <span><i class="iconfont  icon-icon_zhanghao"></i>欢迎您，{{username}}！</span>
                    </el-tooltip>
                </div>
                <div v-else class="top-layout-exit" >
                    <span @click="jumpLogin">暂未登录用户，请登录</span>
                </div>
            </el-header>
            <!--下部-->
            <el-container class="bottom-layout">
                <!--菜单-->
                <el-menu
                        class="el-menu-vertical-demo"
                        @open="handleOpen"
                        @close="handleClose"
                        :collapse="isCollapse"
                        :unique-opened="true"
                        background-color="#FFFFFF"
                        text-color="#7575a3"
                        active-text-color="#3F3C7E"
                        ref="mySidemenu"
                        router
                >
                    <el-submenu v-for="item in menu" :index="item.name" :key="item.name" class="no-boarder">
                        <template slot="title">
                            <i :class="item.meta.icon"></i>&nbsp;&nbsp;
                            <span v-text="item.meta.title"></span>
                        </template>
                        <el-menu-item-group v-for="sub in item.sub" :key="sub.name">
                            <el-menu-item  v-if="sub.meta.active" :index="sub.meta.index" v-text="sub.meta.title">
                            </el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                </el-menu>
                <!--内容-->
                <el-main class="bottom-layout-main">
                    <div class="bottom-layout-nav">
                        <a @click="openclose">
                            <i class="el-icon-s-operation"></i>
                        </a>
                        <div class="top-nav">
                        </div>
                    </div>
                    <div class="bottom-layout-content">
                        <keep-alive>
                            <router-view class="bottom-layout-view"></router-view>
                        </keep-alive>
                    </div>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script>
    import menu from '../config/menu-config'
    export default {
        name: "Basic",
        data() {
            return {
                menu: menu,
                isCollapse: false,
                screenWidth: document.body.clientWidth,
                exit:false,
                username:null
            };
        },
        watch: {
            screenWidth (val) {
                if (!this.timer) {
                    this.screenWidth = val
                    this.timer = true
                    let that = this
                    if(this.screenWidth<1200){
                        this.isCollapse = true
                    }else{
                        this.isCollapse = false
                    }
                    setTimeout(function () {
                        that.timer = false
                    }, 50)
                }
            }
        },
        methods: {
            handleOpen(key, keyPath) {
                console.log(key, keyPath);
            },
            handleClose(key, keyPath) {
                console.log(key, keyPath);
            },
            openclose() {
                this.isCollapse = !this.isCollapse
            },
            jumphome(){
                this.$router.push({path:'/Home'})
            },
            jumpLogin(){
                this.$router.replace({path:'/Login'})
            },
            jumpUserInfo(){
                this.$router.push({path:'/UserInfo'})
            },
            jumpPassword(){
                this.$router.push({path:'/PasswordInfo'})
            },
            exitIt(){
                this.$router.replace({path:'/Login'})
                this.Cookie.remove('loginInfo')
                this.Cookie.remove('pass')
            }
        },
        mounted(){
            if(this.Cookie.getJSON('loginInfo')){
                this.username = this.Cookie.getJSON('loginInfo').username
            }else{
                this.$router.replace({path:'/Login'})
            }
            const that = this
            window.onresize = () => {
                return (() => {
                    window.screenWidth = document.body.clientWidth
                    that.screenWidth = window.screenWidth
                })()
            }

        }
    }

</script>

<style scoped>
    .page {
        width: 100%;
        height: 100%;
        background-color: #E4EFFF;
    }

    .top-layout {
        width: 100%;
        background-color: #3F85ED;
        padding: 0 25px;
        display: flex;
        justify-content: space-between;
    }

    .top-layout-logo {
        display: flex;
        padding-top: 2px;
        cursor: pointer;
    }

    .top-layout-logo img {
        padding-top: 7px;
    }
    .top-layout-exit {
        display: flex;
        justify-content: center;
        align-content: center;
        line-height: 150%;
        padding: 15px;
        color: white;
        cursor: pointer;
    }
    .top-layout-exit i{
        font-size: 1.5em;
        font-weight: bolder;
        margin-right: 8px;
        color: white;
    }
    .top-layout-pop {
        height: 100px;
        width: 110px;
        padding: 0;
        margin: 0;
    }
    .top-layout-pop button {
        height: 35px;
        width: 110px;
        border: none;
        background-color: white;
        font-size: 1.3em;
        cursor: pointer;
    }
    .top-layout-pop button:hover {
        background-color: #dedede;
    }

    .top-layout-title {
        padding: 10px 10px;
        font-size: 1.3em;
        color: white;
        font-weight: bold;
    }
    .top-nav{
        width: 100%;
    }
    .bottom-layout {
        height: 92vh;
        background-color: #E4EFFF;
    }

    i {
        font-size: 1.3em;
        color: #3F3C7E;
    }


    .bottom-layout-main {
        width: 150px;
        padding: 0 20px 20px 20px;
    }

    .bottom-layout-nav {
        padding: 5px 0;
        height: 22px;
        display: flex;
    }

    .bottom-breadcrumb-item i {
        font-size: 1em;
        color: black;
    }

    .bottom-layout-content {
        height: 84.2vh;
        border-radius: 5px;
        background: #fff;

    }

    .bottom-layout-view {
        border-radius: 5px;
        background: #fff;
        padding: 8px 20px;
    }

    .el-menu {
        border: none;
    }

    .el-menu-item.is-active {
        color: #1BC4B1;
        font-weight: bolder;
    }

    >>> .el-menu-item-group__title {
        padding: 0 0 0 20px;
    }

    .el-menu-vertical-demo:not(.el-menu--collapse) {
        width: 200px;
        min-height: 400px;
    }


</style>

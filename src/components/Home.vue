<template>
    <el-container class="home-container">
        <!-- Header -->
        <el-header>
            <span>WCS</span>
            <h3>您好，{{ user.username }}</h3>
            <el-button type="info" @click="userLogout">退出</el-button>
        </el-header>
        <el-container>
            <!-- Aside -->
            <el-aside :width="isCollapse?'64px':'200px'">
                <!-- Toggle button -->
                <div class="toggle-button" @click="toggleCollapse">|||</div>
                <!-- Menu -->
                <el-menu background-color="#333744" text-color="#ffffff" :default-active="$route.path" unique-opened router :collapse="isCollapse" :collapse-transition="false" >
                    <el-menu-item :index="'/'+item.path" v-for="(item,index) in menuList" :key="item.index">
                        <template slot="title">
                            <i :class="iconList[index]"></i>
                            <span>{{ item.name }}</span>
                        </template>
                    </el-menu-item>
                </el-menu>
            </el-aside>
            <!-- Main -->
            <el-main>
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
    export default {
        data(){
            return{
                user: {},
                menuList: [],
                iconList: ["el-icon-user","el-icon-set-up","el-icon-goods","el-icon-s-order","el-icon-s-data","el-icon-s-data"],
                isCollapse: false,
            }
        },
        created(){this.getMyselfInfo()},
        methods: {
			async getMyselfInfo() {
		        const result = await this.$http.get('/info')
                if(result.data.code !== 200) return this.$message.error(result.data.msg)
				// this.$message.success(result.data.msg)
				this.user = result.data.data
                this.getMenuList()
			},
            userLogout:function() {
                window.sessionStorage.clear()
                this.$router.push('/login')
            },
            async getMenuList() {
                const result = await this.$http.get('/menus/user')
                console.log(result)
                if(result.data.code !== 200) return this.$message.error(result.data.msg)
                this.$message.success(result.data.msg)
                this.menuList = result.data.data
            },
            toggleCollapse:function() {
                this.isCollapse = !this.isCollapse
            },
        },
    }
</script>

<style scoped>
    .home-container {width: 100%; height: 100%;}
    .el-header {background-color: #373d41; display: flex; justify-content: space-between; align-items: center; color: #ffffff; font-size: 20px;}
    .el-aside {background-color: #333744;}
    .toggle-button {background-color: #4a5064; font-size: 10px; line-height: 24px; color: #ffffff; text-align: center; letter-spacing: 0.2em; cursor: pointer;}
    .el-menu {border: none;}
    .el-main {background-color: #eaedf1;}
</style>
<template>
  <el-menu
    default-active="1-4-1"
    class="el-menu-vertical-demo"
    @open="handleOpen"
    @close="handleClose"
    :collapse="isCollapse"
    background-color="#545c64"
    text-color="#fff"
    active-text-color="#ffd04b"
  >
  <h3>{{!isCollapse? '通用后台管理系统':'后台'}}</h3>
    <el-menu-item @click="clickMenu(item)" v-for="item in noChildren" :key="item.name" :index="item.name">
      <i :class="`el-icon-${item.icon}`"></i>
      <span slot="title">{{ item.label }}</span>
    </el-menu-item>
    <el-submenu  v-for="item in hasChildren" :key="item.label" :index="item.label">
      <template slot="title">
        <i :class="`el-icon-${item.icon}`"></i>
        <span slot="title">{{item.label}}</span>
      </template>
      <el-menu-item-group  v-for="subItem in item.children" :key ='subItem.path'>
        <el-menu-item @click="clickMenu(subItem)" :index="subItem.path">{{subItem.label}}</el-menu-item>
      </el-menu-item-group>
    </el-submenu>
  </el-menu>
</template>

<style lang="less" scoped>
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  min-height: 400px;
}
.el-menu{
  height: 100vh;
  border-right: 0px;
  h3{
    color: #fff;
    text-align: center;
    line-height: 48px;
    font-size: 16px;
    font-weight: 400;

  }
}

</style>
  

<script>
import Cookie from 'js-cookie';
export default {
  data() {
    return {
      // isCollapse: false,
    };
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    // 点击菜单路由跳转.item为点击当前路由的相关信息
    clickMenu(item){
      if(this.$route.path !== item.path && !(this.$route.path === '/home' && (item.push === '/'))){
        this.$router.push(item.path)
      }
      // 点击触发tab中的函数，传回一个item参数
      this.$store.commit('selectMenu',item) 

    },
  },
  computed:{
    noChildren(){
        return this.menuData.filter(item => !item.children)
    },
    hasChildren(){
        return this.menuData.filter(item => item.children)
    },
    isCollapse(){
      return this.$store.state.tab.isCollapse
    },
    menuData(){
      // 判断当前数据，入宫缓存中没有，就从当前的store中获取
      
      return JSON.parse(Cookie.get('menu')) || this.$store.state.tab.menu 

    },
  }
};
</script>
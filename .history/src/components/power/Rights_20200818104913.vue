<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图 -->
    <el-card>
        <div slot="header"></div>
        <div></div>
    </el-card>
  </div>
</template>
<script>
export default {
    data(){
        return{
            //权限列表
            rightsList:[]
        }
    }
};
</script>
<style lang="less">
</style>
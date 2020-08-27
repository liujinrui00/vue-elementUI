<template>
<div>
  <!-- 面包屑导航区域 -->
  <el-breadcrumb separator-class="el-icon-arrow-right">
    <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
    <el-breadcrumb-item>权限管理</el-breadcrumb-item>
    <el-breadcrumb-item>角色列表</el-breadcrumb-item>
  </el-breadcrumb>
  <!-- 卡片视图 -->
  <el-card>
     <el-table :data="rolesList" border stripe>
            <el-table-column type="index" label="#"></el-table-column>
            <el-table-column  label="角色名称" prop="authName"></el-table-column>
            <el-table-column  label="角色描述" prop="path"></el-table-column>
      </el-table>
  </el-card>
  </div>
</template>
<script>
export default {
    data(){
        return{
            rolesList:[]
        }
    }
};
</script>
<style lang="less">
</style>
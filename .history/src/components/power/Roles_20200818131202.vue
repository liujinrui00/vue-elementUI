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
      <!-- 添加角色按钮区 -->
      <el-row>
        <el-col>
          <el-button type="primary" @click="addRoleDialogVisible = true">添加角色</el-button>
        </el-col>
      </el-row>
      <!-- 角色列表 -->
      <el-table :data="roleList" border stripe>
        <!-- 展开列 -->
        <el-table-column type="expand"></el-table-column>
        <!-- 索引列 -->
        <el-table-column type="index" label="#"></el-table-column>
        <el-table-column label="角色名称" prop="roleName"></el-table-column>
        <el-table-column label="角色描述" prop="roleDesc"></el-table-column>
        <el-table-column label="操作" width="300px">
          <template slot-scope="scope">
            <el-button type="primary" size="mini" icon="el-icon-edit" @click="showEditDialog(scope.row.id)">编辑</el-button>
            <el-button type="danger" size="mini" icon="el-icon-delete" @click="removeRoleById(scope.row.id)">删除</el-button>
            <el-button type="warning" size="mini" icon="el-icon-setting">分配权限</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
    <!-- 添加角色的对话框 -->
    <el-dialog
      title="添加"
      :visible.sync="addRoleDialogVisible"
      width="600px"
      @close="addRoleDialogClosed"
    >
      <!-- 内容主体区域 -->
      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="70px">
        <el-form-item label="角色名称" prop="roleName" label-width="100px">
          <el-input v-model="addForm.roleName"></el-input>
        </el-form-item>
        <el-form-item label="角色描述" prop="roleDesc" label-width="100px">
          <el-input v-model="addForm.roleDesc"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer">
        <el-button @click="addRoleDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addRole">确 定</el-button>
      </span>
    </el-dialog>

    <!--修改角色对话框  -->
    <el-dialog
      title="修改"
      :visible.sync="editRoleDialogVisible"
      width="600px"
      @close="editRoleDialogClosed"
    >
      <!-- 内容主体区域 -->
      <el-form :model="editForm" :rules="editFormRules" ref="editFormRef" label-width="70px">
        <el-form-item label="角色名称" prop="roleName" label-width="100px">
          <el-input v-model="editForm.roleName"></el-input>
        </el-form-item>
        <el-form-item label="角色描述" prop="roleDesc" label-width="100px">
          <el-input v-model="editForm.roleDesc"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer">
        <el-button @click="editRoleDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editRole">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
import { async } from 'q';
export default {
  data() {
    return {
      //角色列表
      roleList: [],
      // 控制添加角色对话框的显示与隐藏
      addRoleDialogVisible: false,
      //添加角色信息
      addForm:{
          roleName:'',
          roelDesc:''
      },
      //添加角色验证规则
      addFormRules:{
          roleName:[{required: true, message:'请输入角色名称', trigger: 'blur'}],
          roleDesc:[{required: true, message:'请输入角色描述', trigger: 'blur'}]
      },
       // 控制修改角色对话框的显示与隐藏
      editRoleDialogVisible: false,
      //修改角色信息
      editForm:{},
      //修改角色验证规则
      editFormRules:{
          roleName:[{required: true, message:'请输入角色名称', trigger: 'blur'}],
          roleDesc:[{required: true, message:'请输入角色描述', trigger: 'blur'}]
      },
    };
  },
  created() {
    //获取角色列表
    this.getRolesList();
  },
  methods: {
    async getRolesList() {
      const { data: res } = await this.$http.get("roles");
      if (res.meta.status !== 200) {
        return this.$message.error("获取角色列表失败");
      }
      this.roleList = res.data;
    },
    addRoleDialogClosed(){
        this.$refs.addFormRef.resetFields()
    },
    //添加角色
    addRole(){
        this.$refs.addFormRef.validate(async valid =>{
            if(!valid) return
            //发送请求
            const {data:res} = await this.$http.post('roles',this.addForm)
            if(res.meta.status !==201){
                this.$message.error('添加角色失败')
            }
            this.$message.success('添加角色成功')
            this.addRoleDialogVisible = false
            this.getRolesList()
        })
    },
    //展示修改角色对话框
    async showEditDialog(id){
        const {data:res} = await this.$http.get('roles/'+ id)
        if(res.meta.status!==200){
            return this.$message.error('查询用户失败')
        }
        this.editForm = res.data
        this.editRoleDialogVisible = true
    },
    //修改对话框关闭，form表单清空
    editRoleDialogClosed(){
        this.$refs.editFormRef.resetFields()
    },
    //修改角色
    editRole(){
        this.$refs.editFormRef.validate(async valid =>{
            if(!valid) return
            //发起修改请求
            const {data:res}=await this.$http.put('roles/' + this.editForm.id,
            {
                roleName:this.editForm.roleName,
                roleDesc:this.editForm.roelDesc
            })
            if(res.meta.status!==200){
                return this.$message.error('更新角色失败')

            }
            //关闭对话框
            this.editRoleDialogVisible = false
            //刷新数据
            this.getRolesList()
            //提示信息
            this.$message.success('更新角色信息成功')
        })
    }
    // //删除角色
    // deleteRole(){}
  }
};
</script>
<style lang="less">
</style>
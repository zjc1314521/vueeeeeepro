<template>
  <div>
    <!-- 面包屑部分 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片部分 -->
    <el-card class="box-card">
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input
            placeholder="请输入内容"
            :clearable="true"
            @clear="getUserInfos"
            @keyup.enter.native="getUserInfos"
            v-model="queryParams.query"
          >
            <el-button slot="append" icon="el-icon-search" @click="getUserInfos"></el-button>
          </el-input>
        </el-col>
        <el-col :span="6">
          <el-button type="primary">添加用户</el-button>
        </el-col>
      </el-row>

      <el-table :data="userInfos" border style="width: 100%">
        <el-table-column type="index" label="序号" width="60"></el-table-column>
        <el-table-column prop="username" label="用户名"></el-table-column>
        <el-table-column prop="mobile" label="手机号" width="140"></el-table-column>
        <el-table-column prop="role_name" label="角色" width="130"></el-table-column>
        <el-table-column prop="email" label="邮箱" width="150"></el-table-column>
        
        <el-table-column prop="mg-state" label="状态" width="70">
          <el-switch slot-scope="info" v-model="info.row.mg_state" @change="changeState(info.row.id,info.row.mg_state)"></el-switch>
        
        </el-table-column>
        
        <el-table-column label="操作" width="270">
          <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
          <el-tooltip  content="分配角色" placement="top" :enterable="false">
            <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
          </el-tooltip>
        </el-table-column>
      </el-table>

      <!-- 分页 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryParams.pagenum"
        :page-sizes="[2, 5,10,15]"
        :page-size="queryParams.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="queryParams.total"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  created() {
    this.getUserInfos()
  },
  methods: {

    handleSizeChange(val) {
      //val:代表变化后的信息条数值
      this.queryParams.pagesize = val
      this.getUserInfos()
    },
    //当前页面回调出来
    handleCurrentChange(val) {
      this.queryParams.pagenum = val
      this.getUserInfos()
    },
    async changeState(id,state){
        const {data:res}=await this.$http.put(`users/${id}/state/${state}`)
              if(res.meta.status!=200){
                return this.$message.error(res.meta.msg)
              }
              this.$message({
                message:res.meta.msg,
                type:"success",
                duration:1500
              })
    },
    async getUserInfos() {
      const { data: res } = await this.$http.get('users', {
        params: this.queryParams
      })
      if (res.meta.status != 200) {
        return this.$message.error(res.meta.msg)
      }
      this.userInfos = res.data.users
      this.queryParams.total = res.data.total
    }
  },
  data() {
    return {
      //搜索关键字
      keywords: '',
      userInfos: [],
      queryParams: {
        query: '', //用于关键字搜索
        pagenum: 1, //当前页码
        pagesize: 3, //没有显示数据
        total: 0
      }
    }
  }
}
</script>

<style lang="less" scoped>
</style>

<template>
  <div>
    <!-- 面包屑部分 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-card>
      <el-table  :data="rightsInfo" border style="width:100%">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column type="index" label="序号"></el-table-column>
        
      </el-table>
      <el-card></el-card>
    </el-card>
  </div>
</template>

<script>
export default {
  created() {
    this.getRightsInfo()
  },
  methods: {
    async getRightsInfo() {
      const { data: res } = await this.$http.get('rights/list')
      this.rightsInfo = res.data
    }
  },
  data() {
    return {
      rightsInfo: ''
    }
  }
}
</script>

<style lang="less" scoped>
</style>

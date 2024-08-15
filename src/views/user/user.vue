<template>
  <div class="dashboard-container">
    <el-form :model="form" label-width="80px" :inline="true" size="small">
      <el-form-item label="活动名称">
        <el-input v-model="form.name" placeholder="请输入用户名称" />
      </el-form-item>
      <el-form-item label="创建时间">
        <el-date-picker v-model="form.minCreateTime" type="datetime" placeholder="起始时间" class="date-picker" />
        <el-date-picker v-model="form.maxCreateTime" type="datetime" placeholder="截止时间" class="date-picker" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini" @click="getUserList">搜索</el-button>
        <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
      </el-form-item>
    </el-form>
    <div>
      <el-button type="primary" icon="el-icon-plus" size="mini" @click="handleCreateUser">新增</el-button>
      <el-button type="danger" icon="el-icon-delete" size="mini" @click="handleBatchDelete">删除</el-button>
      <el-button type="info" icon="el-icon-upload2" size="mini" @click="handleImportUser">导入</el-button>
    </div>

    <!-- 用户列表 -->
    <el-table
      :data="tableData.list"
      @selection-change="val => tableData.selection = val"
      @sort-change="handleSortChange"
    >
      <el-table-column type="index" width="60" />
      <el-table-column type="selection" width="50" />
      <el-table-column width="50">
        <template slot-scope="scope">
          <!-- <img class="table-avatar" :src="scope.row.avatar" /> -->
          <!-- 直接使用:src绑定会导致刷新失败 -->
          <img :id="'avatar-' + scope.row.id" class="table-avatar">
        </template>
      </el-table-column>
      <el-table-column prop="userName" label="用户名" sortable="custom" />
      <el-table-column prop="trueName" label="真实姓名" sortable="custom" />
      <el-table-column prop="roleList" label="角色" sortable="custom" />
      <el-table-column prop="createTime" label="创建时间" sortable="custom" />
      <el-table-column prop="status" label="是否激活" sortable="custom" width="100">
        <template slot-scope="scope">
          <el-switch
            v-model="scope.row.status"
            :active-value="1"
            :inactive-value="0"
            @change="handleSwitch(scope.row)"
          />
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" icon="el-icon-edit" @click="handleEdit(scope.row)">
            编辑 </el-button>
          <el-button
            type="text"
            size="small"
            icon="el-icon-delete"
            style="color: red;"
            @click="handleDelete([scope.row.id])"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'User',
  data() {
    return {
      form: {
        name: '',
        minCreateTime: '',
        maxCreateTime: ''
      },
      tableData: {
        list: [{
          id: 1,
          userName: '张三',
          trueName: '张瑞环',
          roleList: [],
          status: true
        }],
        selection: ''
      }
    }
  }
}
</script>

<style scoped>
.date-picker {
  margin-right: 10px;
  width: 160px;
}
</style>

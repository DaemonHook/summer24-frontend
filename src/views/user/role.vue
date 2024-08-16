<template>
  <div class="dashboard-container">
    <!-- 搜索表单 -->
    <el-form :model="tableData" label-width="80px" :inline="true" size="small">
      <el-form-item label="搜索内容">
        <el-input v-model="tableData.name" placeholder="请输入搜索内容" clearable="" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini" @click="getRoleList">搜索</el-button>
        <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
      </el-form-item>
    </el-form>

    <!-- 角色列表 -->
    <el-table
      :data="tableData.list"
      @sort-change="handleSortChange"
    >
      <el-table-column type="index" width="60" />
      <el-table-column prop="name" label="角色名称" sortable="custom" />
      <el-table-column prop="description" label="用户描述" sortable="custom" />
      <el-table-column prop="createTime" label="创建时间" sortable="custom" />
      <el-table-column prop="updateTime" label="更新时间" sortable="custom" />
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

    <!-- 分页 -->
    <el-pagination
      class="pagination"
      :current-page.sync="tableData.pageNum"
      :page-sizes="[10, 20, 30, 40]"
      :page-size.sync="tableData.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="tableData.total"
      @size-change="getRoleList"
      @current-change="getRoleList"
    />

    <!-- 用户编辑/创建窗口 -->
    <el-dialog
      class="role-edit-dialog"
      :title="roleEditForm.id ? '新增角色' : '角色编辑'"
      :visible.sync="roleEditDialogVisible"
      width="50%"
    >
      <el-form
        ref="roleEditForm"
        status-icon
        :model="roleEditForm"
        :rules="roleEditFormRules"
        label-width="80px"
      >
        <el-form-item label="角色名称" prop="name">
          <el-input v-model="roleEditForm.name" />
        </el-form-item>
        <el-form-item label="角色描述" prop="description">
          <el-input v-model="roleEditForm.description" />
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="roleEditDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addOrUpdateRole">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'Role',
  data() {
    return {
      tableData: {
        searchContent: '',
        list: [{
          id: 1,
          name: '达尔',
          description: '描述',
          createTime: '',
          updateTime: ''
        }],
        pageNum: 1,
        pageSize: 10,
        total: 1
      },
      roleEditForm: {
        id: '',
        name: '',
        description: ''
      },
      roleEditFormRules: {
        name: [
          { required: true, trigger: 'blur', validator: this.validateName }
        ]
      },
      roleEditDialogVisible: false
    }
  },
  methods: {
    // validateName(role, value, callback) {
    //   if (!value) {
    //     callback(new Error('请输入角色名称'))
    //   } else {
    //     if (this.roleEditForm.id && this.roleEditForm.value === value) {
    //       callback()
    //     }
    // },
    // 新增角色
    handleCreateRole() {
      for (const key in this.roleEditForm) {
        this.roleEditForm[key] = ''
      }
      this.openRoleEditDialog()
    },
    // 打开角色编辑入口
    openRoleEditDialog() {
      this.roleEditDialogVisible = true
      this.$nextTick(() => {
        this.$ref.roleEditForm.clearValidate()
      })
    },
    // 删除角色
    handleDelete() {
      this.$confirm('此操作将永久删除角色，是否继续？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
      }).catch(() => {
        this.$message.info('已取消删除')
      })
    },
    getRoleList() {
      return this.tableData.list
    },
    resetQuery() {
    },
    handleSortChange() {
    },
    addOrUpdateRole() {
    }
  }
}

</script>

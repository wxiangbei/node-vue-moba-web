<template>
  <div>
    <h1>文章列表</h1>
    <el-table :data="items">
      <el-table-column prop="_id"
                       label=ID
                       width="220">
      </el-table-column>
      <el-table-column prop="title"
                       label="文章标题">
      </el-table-column>
      <el-table-column fixed="right"
                       label="操作"
                       width="180">
        <template slot-scope="scope">
          <el-button @click="$router.push(`/articles/edit/${scope.row._id}`)"
                     type="text"
                     size="small">编辑</el-button>
          <el-button @click="remove(scope.row)"
                     type="text"
                     size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: []
    }
  },
  created () {
    this.fetch()
  },
  methods: {
    async fetch () {
      const res = await this.$http.get('rest/articles')
      this.items = res.data
    },
    async remove (row) {
      this.$confirm(`确认删除：${row.title}`, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        await this.$http.delete(`rest/articles/${row._id}`)
        let index = this.items.findIndex(item => item._id == row._id)
        this.items.splice(index, 1)
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      })
    }
  },
}
</script>

<style>
</style>
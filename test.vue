<template>
  <div>
    <!-- 表格 -->
    <el-table
      ref="table"
      :data="datas"
    >
      <el-table-column
        prop="id"
        label="ID"
      />
      <el-table-column
        prop="settle_amount"
        label="实际金额"
      />
      <el-table-column label="操作" min-width="160" align="right" fixed="right">
        <template slot-scope="{ row }">
          <el-button size="mini" type="text" @click="examine(row)">审核</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 审核对话框 -->
    <el-dialog
      :visible.sync="dialog"
      :before-close="close"
    >
      <el-form
        ref="ref"
        :model="model"
        :rules="rules"
      >
        <el-form-item label="实际金额" prop="settle_amount">
          {{ model.settle_amount }}
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="close">取 消</el-button>
        <el-button
          type="primary"
          @click="submit()"
        >确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'AuthUser',
  data() {
    return {
      datas: [],
      dialog: false,
      // 审核内容
      model: {
        id: 0,
        settle_amount: 0
      },
      rules: {}
    }
  },
  created() {
    // 表格数据
    for (let i = 0; i < 10; i++) {
      this.datas.push({
        id: parseInt(Math.random() * 10),
        settle_amount: parseFloat(Math.random() * 100).toFixed(2)
      })
    }
  },
  methods: {
    // 审核
    examine(row) {
      this.dialog = true
      this.set(row)
    },
    // 对话框内容设置
    set(row) {
      if (row) {
        this.model.id = row.id
        this.model.settle_amount = row.settle_amount
      } else {
        this.model = this.$options.data().model
      }
      if (this.$refs['ref'] !== undefined) {
        console.log('清除')
        this.$refs['ref'].resetFields()
      }
    },
    // 关闭对话框
    close() {
      this.dialog = false
      this.set()
    },
    // 提交
    submit() {
      this.$refs['ref'].validate((valid) => {
        if (valid) {
          console.log(JSON.stringify(this.model))
          return
        }
      })
    }
  }
}
</script>


<template>
  <section>
    <el-col
      :span="24"
      class="toolbar"
      style="padding-bottom: 0; text-align: left"
    >
      <el-form :inline="true" :model="filters">
<!--        <el-form-item>
          <el-input v-model="filters.reporter" placeholder="作者"></el-input>
        </el-form-item>-->


<!--起止时间查询开始-->
        <el-form-item label="起始时间" prop="date">
          <!--<el-input v-model="addForm.publishDate"></el-input>-->
          <el-date-picker
            v-model="filters.beginDate"
            name="beginDate"
            type="date"
            placeholder="选择日期"
            format="yyyyMMdd"
            value-format="yyyyMMdd"
          ></el-date-picker>
        </el-form-item>
        <el-form-item label="终止时间" prop="date">
          <!--<el-input v-model="addForm.publishDate"></el-input>-->
          <el-date-picker
            v-model="filters.overDate"
            name="overDate"
            type="date"
            placeholder="选择日期"
            format="yyyyMMdd"
            value-format="yyyyMMdd"
          ></el-date-picker>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="retrieveStatementListByRangeDate">查询</el-button>
        </el-form-item>
<!--起止时间查询结束-->


<!--单日查询开始-->
        <el-form-item label="" prop="date">
          <!--<el-input v-model="addForm.publishDate"></el-input>-->
          <el-date-picker
            v-model="filters.singleDate"
            name="overDate"
            type="date"
            placeholder="选择日期"
            format="yyyyMMdd"
            value-format="yyyyMMdd"
          ></el-date-picker>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="retrieveStatementListBySingleDate">查询</el-button>
        </el-form-item>
<!--单日查询结束-->



        <el-form-item>
          <el-button type="primary" @click="handleAdd">新增</el-button>
        </el-form-item>

      </el-form>
    </el-col>

    <el-col :span="24" style="background: #ff0">
      <el-table
        border
        :data="statements"
        :loading="listLoading"
        @selection-change="selectionRowsChange"
        style="width: 100%; text-align: left;"
      >
        <el-table-column type="selection" min-width="50"></el-table-column>
        <el-table-column
          type="index"
          label="#"
          min-width="50"
        ></el-table-column>
        
        <el-table-column
          prop="date"
          label="日期"
          min-width="50"
        ></el-table-column>
        <el-table-column
          prop="reporter"
          label="报告人"
          min-width="50"
        ></el-table-column>
        <el-table-column
          prop="title"
          label="标题"
          min-width="100"
        ></el-table-column>
        <el-table-column
          prop="content"
          label="内容"
          min-width="300"
        ></el-table-column>
        <el-table-column prop="attachment" label="附件" min-width="200">
          <template slot-scope="scope">
            <a
              :href="'http://localhost:9080/attachment/' + scope.row.attachment"
              target="_blank"
              class="buttonText"
            >{{ scope.row.attachment }}</a
            >
          </template>
        </el-table-column>

        <el-table-column label="操作" min-width="200">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="danger"
              @click="deleteStatementById(scope.$index, scope.row)"
            >删除</el-button
            >
            <el-button
              size="mini"
              type="danger"
              @click="handleUpdate(scope.$index, scope.row)"
            >编辑</el-button
            >
          </template>
        </el-table-column>
      </el-table>
    </el-col>

    <el-col :span="24" style="margin-top: 20px; text-align: left">
      <el-pagination
        background
        layout="total, prev, pager, next, jumper"
        :total="total"
        :page-size="pageSize"
        @current-change="handleCurrentChange"
        @size-change="handleSizeChange"
        :current-page.sync="currentPage"
        style="float: right;"
      ></el-pagination>
    </el-col>


    <el-dialog
      title="新增工作报告"
      :visible.sync="dialogCreateVisible"
      style="text-align: left"
    >
      <el-form
        :rules="statementRules"
        ref="statement"
        :model="statement"
        :label-width="statementLabelWidth"
      >
        <el-form-item label="报告日期" prop="date">
          <!--<el-input v-model="addForm.publishDate"></el-input>-->
          <el-date-picker
            v-model="statement.date"
            name="date"
            type="date"
            placeholder="选择日期"
            format="yyyyMMdd"
            value-format="yyyyMMdd"
          ></el-date-picker>
        </el-form-item>
        <el-form-item label="标题" prop="title">
          <el-input
            v-model="statement.title"
            name="title"
            auto-complete="off"
          ></el-input>
        </el-form-item>
        <el-form-item label="内容" prop="content">
          <el-input
            type="textarea"
            :rows="7"
            v-model="statement.content"
          ></el-input>
        </el-form-item>
        <el-form-item label="附件" prop="attachment">
          <input type="file" @change="handleFileChange($event, 'attachment')" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="createReset">取消</el-button>
        <el-button
          @click.native="createSubmit"
          :loading="addLoading"
          type="primary"
        >确定</el-button
        >
      </div>
    </el-dialog>

<el-dialog
      title="编辑工作报告"
      :visible.sync="dialogUpdateVisible"
      style="text-align: left"
    >
      <el-form
        :rules="statementRules"
        ref="statement"
        :model="statement"
        :label-width="statementLabelWidth"
      >
        <el-form-item label="报告日期" prop="date">
          <!--<el-input v-model="addForm.publishDate"></el-input>-->
          <el-date-picker
            v-model="statement.date"
            name="date"
            type="date"
            placeholder="选择日期"
            format="yyyyMMdd"
            value-format="yyyyMMdd"
          ></el-date-picker>
        </el-form-item>
        <el-form-item label="标题" prop="title">
          <el-input
            v-model="statement.title"
            name="title"
            auto-complete="off"
          ></el-input>
        </el-form-item>
        <el-form-item label="内容" prop="content">
          <el-input
            type="textarea"
            :rows="7"
            v-model="statement.content"
          ></el-input>
        </el-form-item>
        <el-form-item label="附件" prop="attachment">
          <input type="file" @change="handleFileChange($event, 'attachment')" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="createReset">取消</el-button>
        <el-button
          @click.native="updateSubmit(statement)"
          :loading="addLoading"
          type="primary"
        >确定</el-button
        >
      </div>
    </el-dialog>



  </section>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      statements: [],
      filters: {
        userName: ''
        },
      total: 0,
      pageSize: 5,
      currentPage: 1,
      listLoading: false,
      sels: [],

      statement: {
        date: '',
        title: '',
        content: '',
        attachment: ''
      },
      statementRules: {
        date: [{ required: true, message: '请选择报告日期', trigger: 'blur' }],
        title: [{ required: true, message: '请输入报告标题', trigger: 'blur' }],
        content: [
          { required: true, message: '请输入报告内容', trigger: 'blur' }
        ]
      },
      dialogCreateVisible: false,
      dialogUpdateVisible: false,
      statementLabelWidth: '80px',
      addLoading: false
    }
  },


//*****填充方法*****/
  methods: {
    handleCurrentChange(val) {
      this.currentPage = val
      this.retrieveAllStatementList()
    },

    handleSizeChange(val) {
      this.pageSize = val
      this.retrieveAllStatementList()
    },

//添加+编辑--置空
    createReset() {
      this.dialogCreateVisible = false
      this.dialogUpdateVisible = false
      this.$refs.statement.resetFields()
    },

//新增--设置默认值
    handleAdd() {
      this.dialogCreateVisible = true
      this.statement = {
        date: '',
        title: '',
        content: '',
        attachment: ''
      }
    },
//更新--设置默认值
    handleUpdate(index,row) {
      this.dialogUpdateVisible = true
      //this.statement = {
      //  date: '',
      //  title: '',
      //  content: '',
      //  attachment: ''
      //},
      console.log(index, row)
      //row是该行tableData对应的一行
      this.statement = row
    },


//添加 + 编辑  --将文件存至本地
  handleFileChange(event, type) {
      console.log(this.statement[type])
      this.addLoading = true
      let config = {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      }

      let formData = new FormData()
      formData.append('file', event.target.files[0])

      this.$axios
        .post('http://localhost:9080/attachment', formData, config)
        .then(res => {
          this.statement[type] = res.data
          console.log(res.data)
        })
        .catch(err => {
          console.log(err)
        })
      this.addLoading = false
    },

//添加--确定按钮--数据库
    createSubmit() {
      this.$refs.statement.validate(valid => {
        if (valid) {
          this.$confirm('确认提交？', '提示', {})
            .then(() => {
              this.addLoading = true

              // let addPara = new URLSearchParams()
              // addPara.append('date', this.statement.date)
              // addPara.append('title', this.statement.title)
              // addPara.append('content', this.statement.content)
              // addPara.append('attachment', this.statement.attachment)

              this.$axios
                .post('http://localhost:9080/statement/add', this.statement)
                .then(res => {
                  console.log(res.data)
                  this.addLoading = false
                  this.$message({
                    type: 'success',
                    message: '保存成功!'
                  })
                  this.dialogCreateVisible = false
                  this.retrieveAllStatementList()
                })
                .catch(err => {
                  console.log(err)
                })
            })
            .catch(() => {
              this.$message({
                type: 'info',
                message: '已取消'
              })
            })
        } else {
          console.log('error')
          return false
        }
      })
    },

//更新--文件--添加已合并

//更新--根据ID
    updateStatementById(updateScope) {
      
      console.log(updateScope)
      this.$confirm('确定更新吗？ ', '提示', {
        type: 'warning'
      }).then(() => {
        this.listLoading = true

        this.$axios
          .post('http://localhost:9080/statement/update' + updateScope.row.id, updateScope.statement)
          .then(res => {
            let isSuccess = res.data.success

            if (isSuccess == true) {
              this.$message({
                message: '更新成功',
                type: 'success'
              })
            } else {
              this.$message({
                message: res.data.message,
                type: 'error'
              })
            }

            this.retrieveAllStatementList()
            this.listLoading = false
          })
          .catch(err => {
            console.log(err)
          })
      }).catch(err => {
          console.log(err)
      })
    },
//更新--数据库
    updateSubmit(statement) {
      this.$refs.statement.validate(valid => {
        if (valid) {
          this.$confirm('确认提交？', '提示', {})
            .then(() => {
              this.addLoading = true

              // let addPara = new URLSearchParams()
              // addPara.append('date', this.statement.date)
              // addPara.append('title', this.statement.title)
              // addPara.append('content', this.statement.content)
              // addPara.append('attachment', this.statement.attachment)

              this.$axios
                .post('http://localhost:9080/statement/update' + statement.id, this.statement)
                .then(res => {
                  console.log(res.data)
                  this.addLoading = false
                  this.$message({
                    type: 'success',
                    message: '编辑成功!'
                  })
                  this.dialogUpdateVisible = false
                  this.retrieveAllStatementList()
                })
                .catch(err => {
                  console.log(err)
                })
            })
            .catch(() => {
              this.$message({
                type: 'info',
                message: '已取消'
              })
            })
        } else {
          console.log('error')
          return false
        }
      })
      this.retrieveAllStatementList()
    },
  

//查询--所有列表展示
  retrieveAllStatementList(){
      let para = new URLSearchParams()
      para.append('singleDate', this.filters.singleDate)
      para.append('currentPage', this.currentPage)
      para.append('pageSize', this.pageSize)
      this.listLoading = true

      this.$axios
        .get('http://localhost:9080/statement/findAll', {
          params: para
        })
        .then(res => {
          this.total = res.data.total
          this.statements = res.data.rows
          this.listLoading = false
        })
        .catch(err => {
          console.log(err)
        })
    },



//查询--日期范围
    retrieveStatementListByRangeDate() {
      let para = new URLSearchParams()
      para.append('beginDate', this.filters.beginDate)
      para.append('overDate', this.filters.overDate)
      para.append('currentPage', this.currentPage)
      para.append('pageSize', this.pageSize)
      this.listLoading = true

      this.$axios
        .get('http://localhost:9080/statement/beginDate', {
          params: para
        })
        .then(res => {
          this.total = res.data.total
          this.statements = res.data.rows
          this.listLoading = false
        })
        .catch(err => {
          console.log(err)
        })
    },



//查询--单日
    retrieveStatementListBySingleDate(){
      let para = new URLSearchParams()
      para.append('singleDate', this.filters.singleDate)
      para.append('currentPage', this.currentPage)
      para.append('pageSize', this.pageSize)
      this.listLoading = true

      this.$axios
        .get('http://localhost:9080/statement/singleDate', {
          params: para
        })
        .then(res => {
          this.total = res.data.total
          this.statements = res.data.rows
          this.listLoading = false
        })
        .catch(err => {
          console.log(err)
        })
    },

//根据ID删除
    deleteStatementById(index, row) {
      this.$confirm('确定删除吗？ ', '提示', {
        type: 'warning'
      }).then(() => {
        this.listLoading = true

        this.$axios
          .delete('http://localhost:9080/statement/delete' + row.id, {})
          .then(res => {
            let isSuccess = res.data.success

            if (isSuccess == true) {
              this.$message({
                message: '删除成功',
                type: 'success'
              })
            } else {
              this.$message({
                message: res.data.message,
                type: 'error'
              })
            }

            this.retrieveAllStatementList()
            this.listLoading = false
          })
          .catch(err => {
            console.log(err)
          })
      })
    },

    selectionRowsChange(sels) {
      this.sels = sels
    }
  },


//
  mounted() {
    this.retrieveAllStatementList()
  }
}
</script>

<style scoped type="scss">
.el-table td {
  text-align: left;
}
</style>

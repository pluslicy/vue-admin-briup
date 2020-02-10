<template>
  <div>
    <!-- 按钮 -->
    <div class="btns">
      <el-button size="small" type="primary" @click="toAddHandler">新增</el-button>
    </div>
    <!-- /按钮 -->
    <!-- 表格 -->
    <el-table :data="categories" style="width: 100%">
      <el-table-column prop="id" label="编号" width="180"> </el-table-column>
      <el-table-column prop="name" label="栏目名称" width="180"> </el-table-column>
      <el-table-column prop="num" label="序号"> </el-table-column>
      <el-table-column prop="icon" label="图标"> </el-table-column>
      <el-table-column label="操作" width="120" align="center"> 
        <template slot-scope="scope">
          <a href="" @click.prevent="deleteHandler(scope.row.id)">删除</a>
          <a href="" @click.prevent="toUpdate(scope.row)">修改</a>
        </template>
      </el-table-column>
    </el-table>
    <!-- /表格 -->
    <!-- 分页 -->
    <!-- /分页 -->
    <!-- 模态框 -->
    <el-dialog
      :title="title"
      :visible.sync="visible"
      width="60%">
      <!-- 表单 -->
      {{form}}
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="栏目名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="序号">
          <el-input v-model="form.num"></el-input>
        </el-form-item>
      </el-form>

      <span slot="footer" class="dialog-footer">
        <el-button @click="visible = false">取 消</el-button>
        <el-button type="primary" @click="submitHandler">确 定</el-button>
      </span>
    </el-dialog>
    <!-- /模态框 -->
  </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
  data(){
    return {
      categories:[],
      visible:false,
      form:{},
      title:"新增栏目"
    }
  },
  created(){
    this.loadCategories();
  },
  methods:{
    // 修改
    toUpdate(row){
      this.title = "更新栏目";
      this.form = row;
      this.visible = true;
    },
    // 提交表单
    submitHandler(){
      request({
        url:"/category/saveOrUpdate",
        method:"POST",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data:querystring.stringify(this.form)
      }).then(result =>{
        this.visible = false;
        this.$message({ type: 'success', message: result.message });
        this.loadCategories();
      })
    },
    // 删除功能
    deleteHandler(id){
      // 询问
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 删除
        request.get("/category/deleteById?id="+id)
        .then(result =>{
          // 删除成功
          this.$message({ type: 'success', message: result.message });
          // 重载数据
          this.loadCategories();
        })
      })
    },
    // 加载栏目信息
    loadCategories(){
      request.get('/category/findAll').then(result=>{
        this.categories = result.data;
      })
    },
    toAddHandler(){
      this.title = "新增栏目";
      this.form = {};
      this.visible = true;
    }
  }
}
</script>
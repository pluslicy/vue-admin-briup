<template>
  <div>
    <!--按钮-->
    <div>
      <el-button @click="toAddHandler">添加</el-button>
      <el-button>批量删除</el-button>
    </div>
    <!--表格-->
    <div>
      <el-table style="width: 100%" :data="products">
        <el-table-column prop="id" label="编号" width="180"> </el-table-column>
        <el-table-column prop="name" label="名称" width="180"> </el-table-column>
        <el-table-column prop="price" label="单价"> </el-table-column>
      </el-table>
    </div>
    <!--模态框-->
    <el-dialog :visible="visible" title="录入产品">

      <span slot="footer" class="dialog-footer">
        <el-button @click="visible = false">取 消</el-button>
        <el-button type="primary" @click="visible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import request from '@/utils/request'
export default {
  data(){
    return {
      visible:false,
      products:[]
    }
  },
  // 当vue实例创建完毕
  created(){
    this.loadCategories();
  },
  methods:{
    toAddHandler(){
      this.visible = true;
    },
    loadCategories(){
      let url ="http://134.175.154.93:6677/product/findAll"
      request.get(url).then((result)=>{
        console.log(result);
        this.products = result.data;
      })
    }
  }
}
</script>

<style scoped>

</style>
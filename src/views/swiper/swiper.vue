<template>
<div class="swiper-all">
    <el-table
    :data="formData"
    style="width: 100%">
    <el-table-column
      fixed
      prop="img"
      label="轮播图图片"
      width="180">
      <template slot-scope="scope">   
          <img :src="scope.row.img" width="80" height="100">
      </template>
    </el-table-column>
    <el-table-column
      prop="title"
      label="轮播图标题"
      width="180">
    </el-table-column>
    <el-table-column
      prop="img"
      label="书籍封面"
      width="180">
       <template slot-scope="scope">   
          <img :src="scope.row.book.img" width="80" height="100">
      </template>
    </el-table-column>
    <el-table-column
      prop="book.title"
      label="书名"
      width="180">
    </el-table-column>
    <el-table-column
      fixed="right"
      label="操作"
      width="180">
      <template slot-scope="scope">
        <el-button @click="handleClick(scope.row._id)" type="primary" size="small">编辑</el-button>
        <el-button @click="handleDelete(scope.row._id)" type="danger" size="small">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-pagination
  :page-size="5"
    background
@current-change="pageChange"
  layout="prev, pager, next"
  :total="count">
</el-pagination>
     </div>
     
</template>
<script>
export default {
  
   data(){
       return{
           formData:[],
           pn:1,
           size:5,
           count:50
       }
   },
   methods:{
       getData(){
           this.$axios.get('/swiper',{pn:this.pn,size:this.size}).then(res=>{
               this.formData=res.data
               this.count=res.count
               console.log(res)
           }).catch(
           )
       },
       pageChange(page){
this.pn=page
this.getData()
       },
         handleClick(id) {
this.$router.push({name:'editSwiper',query:{id}})
      },
      handleDelete(id) {
              this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
             this.$axios.post(`swiper/delete`,{ids:[id]}).then(res=>{
           this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.getData()
          })
         
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
         
      }
   
   } ,
        created(){
            this.getData()
   }
  
}
</script>

<template >
  <!-- <el-table
    :data="tableData3"
    style="width: 100%"
    height="250">
    <el-table-column
      fixed
      prop="date"
      label="日期"
      width="150">
    </el-table-column>
    <el-table-column
      prop="name"
      label="姓名"
      width="120">
    </el-table-column>
    <el-table-column
      prop="province"
      label="省份"
      width="120">
    </el-table-column>
    <el-table-column
      prop="city"
      label="市区"
      width="120">
    </el-table-column>
    <el-table-column
      prop="address"
      label="地址"
      width="300">
    </el-table-column>
    <el-table-column
      prop="zip"
      label="邮编"
      width="120">
    </el-table-column>
  </el-table> -->
</template>
<script>
export default {
    data(){
        return{
            formData:{},
            bookList:{},
            bookListIds:[],
            books:[],
            allbooks:[]
        }
    },
    methods:{
getBookLists(){
    this.$axios.get('category',{size:100}).then(res=>{
        this.bookList=res.data
        let arr=[]
        res.data.forEach(item=>{
            arr.push(item._id)
        })
        this.bookListIds=arr
        this.getBooks()
    })
},
getBooks(){
    this.bookListIds.map(item=>{
  this.$axios.get(`category/${item}/books`).then(res=>{
res.data.books.forEach(item=>{
    this.books.push(item)
})
  })
    })
  console.log(this.books)
//   console.log(Object.keys(this.books))
// for(let i in this.books){
//     console.log(i)
// }
}
    },created(){
        this.getBookLists()
    }
}
</script>


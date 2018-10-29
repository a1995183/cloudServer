<template>
    <div>
        <el-form :model="formData" class="add-book">
            <el-form-item label="网址">
                <el-input v-model="formData.url"></el-input>
            </el-form-item>
            <el-form-item label="作者">
                <el-input v-model="formData.author"></el-input>
            </el-form-item>
            <el-form-item label="书籍封面">
                <el-switch
                v-model="isUpload"
                active-text="手动上传"
                inactive-text="封面图网址"
                >
                </el-switch>
                <el-input v-model="formData.img" v-if="!isUpload"></el-input>
                <upload-img v-model="formData.img" v-else></upload-img>
            </el-form-item>
            <el-form-item label="分类">
                <el-select v-model="formData.typeId">
                    <el-option v-for="(item,index) in categoryData"
                    :key="index"
                    :label="item.title"
                    :value="item._id">
                    </el-option>
                </el-select>
            </el-form-item>
        </el-form>  
        <el-button @click="handleSubmit">提交</el-button>
    </div>
</template>
<script>
import uploadImg from '@/components/upload-com'
export default {
    name:"addBook",
      components:{
uploadImg
    },
    data(){
        return{
            formData:{
                url:'',
                author:'',
                img:'',
                typeId:''
            },
            isUpload:false,
            categoryData:[]
        }
    },
    methods:{
        async getCategoryData(){
            const res=await this.$axios.get('/category',{size:100})
            this.categoryData=res.data
        },
        handleSubmit(){
            this.$axios.post('/book',this.formData).then(res=>{
                if(res.code==200){
                    this.$message.success(res.msg)
                }
            })
        }
    },created(){
        this.getCategoryData()
    }
}
</script>
<style scoped lang="scss">
.add-book{
    /deep/{
        .el-input__inner{
width:500px;
        }
    }
}
</style>


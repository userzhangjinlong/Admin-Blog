<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 分类管理</el-breadcrumb-item>
                <el-breadcrumb-item>添加分类</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" :rules="rules" label-width="80px">
                    <el-form-item label="分类名称">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="选择父级">
                        <el-select v-model="form.pid" placeholder="请选择">
                            <el-option key="0" label="一级分类" value="0"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmit('rules')">添加文章</el-button>
                        <el-button>取消</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'baseform',
        data: function(){
            return {
                form: {
                    name: '',
                    pid: '',
                },
                rules: {
                    name: [
                        { required: true, message: '请输入分类名称', trigger: 'blur' }
                    ],
                    pid: [
                        { required: true, message: '请选择分类等级', trigger: 'change' }
                    ]
                }
            }
        },
        methods: {
            onSubmit() {
                 /*this.$refs[formName].validate((valid) => {*/
                    let url = domain.testUrl+'/categoryAdd';
                    /*if(valid){*/
                        this.$axios.post(url,{
                            name:this.form.name,
                            pid:this.form.pid
                        }).then(response => {
                            console.log(response.data.code)
                            if(response.data.code == 200){
                                this.$message.success('添加成功！');
                                //跳转分类列表
                                /*setTimeout(function() {
                                    window.location.href = '/BaseCategoryList';
                                }, 2000);*/
                            }else{
                                this.$message.success(response.data.msg);
                            }
                            // resolve(response.data);
                        }).catch((error) => {
                            console.log(error)
                            reject(error)
                        });
                    /*}else{
                        this.$message.error('添加失败！');
                        return false;
                    }*/
                /*});*/

            }
        }
    }
</script>
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
                            <el-option key="bbk" label="步步高" value="bbk"></el-option>
                            <el-option key="xtc" label="小天才" value="xtc"></el-option>
                            <el-option key="imoo" label="imoo" value="imoo"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmit">添加文章</el-button>
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
                // console.log(this.$refs);return false;
                /*this.$refs[rules].validate((valid) => {*/
                    console.log(this.form);
                    this.$message.success('提交成功！');
                /*});*/

            }
        }
    }
</script>
<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i>文章管理</el-breadcrumb-item>
                <el-breadcrumb-item>添加文章</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" :rules="rules" label-width="80px">
                    <el-form-item label="标题">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="分类">
                        <el-select v-model="form.cat" placeholder="请选择">
                            <el-option key="bbk" label="步步高" value="bbk"></el-option>
                            <el-option key="xtc" label="小天才" value="xtc"></el-option>
                            <el-option key="imoo" label="imoo" value="imoo"></el-option>
                        </el-select>
                    </el-form-item>


                    <!--<el-form-item label="选择开关">
                        <el-switch v-model="form.delivery"></el-switch>
                    </el-form-item>-->

                    <el-form-item label="描述内容">
                        <el-input type="textarea" rows="5" v-model="form.desc"></el-input>
                    </el-form-item>
                    <el-form-item label="详情">
                    </el-form-item>
                    <el-form-item class="container" style="width:200%">
                        <div class="plugins-tips">
                            Vue-Quill-Editor：基于Quill、适用于Vue2的富文本编辑器。
                            访问地址：<a href="https://github.com/surmon-china/vue-quill-editor" target="_blank">vue-quill-editor</a>
                        </div>
                        <quill-editor ref="myTextEditor" v-model="form.content" ></quill-editor>
                        <!--:options="editorOption"-->
                    </el-form-item>

                    <el-form-item>
                        <el-button type="primary" @click="onSubmit">表单提交</el-button>
                        <el-button>取消</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    import 'quill/dist/quill.core.css';
    import 'quill/dist/quill.snow.css';
    import 'quill/dist/quill.bubble.css';
    import { quillEditor } from 'vue-quill-editor';
    export default {
        name: 'baseform',
        data: function(){
            return {
                options:[
                    {
                        value: 'guangdong',
                        label: '广东省',
                        children: [
                            {
                                value: 'guangzhou',
                                label: '广州市',
                                children: [
                                    {
                                        value: 'tianhe',
                                        label: '天河区'
                                    },
                                    {
                                        value: 'haizhu',
                                        label: '海珠区'
                                    }
                                ]
                            },
                            {
                                value: 'dongguan',
                                label: '东莞市',
                                children: [
                                    {
                                        value: 'changan',
                                        label: '长安镇'
                                    },
                                    {
                                        value: 'humen',
                                        label: '虎门镇'
                                    }
                                ]
                            }
                        ]
                    },
                    {
                        value: 'hunan',
                        label: '湖南省',
                        children: [
                            {
                                value: 'changsha',
                                label: '长沙市',
                                children: [
                                    {
                                        value: 'yuelu',
                                        label: '岳麓区'
                                    }
                                ]
                            }
                        ]
                    }
                ],
                form: {
                    name: '',
                    cat: '',
                    desc: '',
                    content: ''
                    /*editorOption: {
                        placeholder: 'Hello World'
                    }*/
                },
                rules:{
                    name:[
                        { required: true, message: '请输入文章标题', max:'120',trigger: 'blur' }
                    ],
                    cat:[
                        { required:true, message:'请选择分类', trigger:'change' }
                    ],
                    content:[
                        { required:true, message:'请输入内容'}
                    ]

                }
            }
        },
        components: {
            quillEditor
        },
        methods: {
            onSubmit() {
                console.log(this.form);
                this.$message.success('提交成功！');
            }
        }
    }
</script>
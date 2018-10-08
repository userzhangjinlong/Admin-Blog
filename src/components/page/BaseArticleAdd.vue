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
        computed: {
            data() {
                return this.tableData.filter((d) => {
                    let is_del = false;
                    for (let i = 0; i < this.del_list.length; i++) {
                        if (d.name === this.del_list[i].name) {
                            is_del = true;
                            break;
                        }
                    }
                    if (!is_del) {
                        if (d.address.indexOf(this.select_cate) > -1 &&
                            (d.name.indexOf(this.select_word) > -1 ||
                                d.address.indexOf(this.select_word) > -1)
                        ) {
                            return d;
                        }
                    }
                })
            }
        },
        components: {
            quillEditor
        },
        mounted() {
            this.getData();
        },
        methods: {
            // 获取分类数据
            getData() {
                // 开发环境使用 easy-mock 数据，正式环境使用 json 文件
                /*if (process.env.NODE_ENV === 'development') {
                    this.url = '/ms/table/list';
                };*/
                let url = domain.testUrl+'/categoryList';
                this.$axios.get(url, {
                }).then((res) => {
                    console.log(res);return false;
                    this.tableData = res.data.list;
                })
            },
            onSubmit() {
                console.log(this.form);
                this.$message.success('提交成功！');
            }
        }
    }
</script>
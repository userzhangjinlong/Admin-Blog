<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i>文章管理</el-breadcrumb-item>
                <el-breadcrumb-item>添加文章</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container" @current-load="getData">
            <div class="form-box">
                <el-form ref="form" :model="form" :rules="rules" label-width="80px">
                    <el-form-item label="标题">
                        <el-input v-model="form.title"></el-input>
                    </el-form-item>
                    <el-form-item label="分类">
                        <el-select v-model="form.cat_id" placeholder="请选择" >
                            <el-option v-for="cat in cateList"  :key="cat.id" :label="cat.name" :value="cat.id" >{{ cat.name }}</el-option>
                        </el-select>
                    </el-form-item>



                    <!--<div class="plugins-tips">
                        vue-cropperjs：一个封装了 cropperjs 的 Vue 组件。
                        访问地址：<a href="https://github.com/Agontuk/vue-cropperjs" target="_blank">vue-cropperjs</a>
                    </div>
                    <div class="crop-demo">
                        <img :src="cropImg" class="pre-img">
                        <div class="crop-demo-btn">选择图片
                            <input class="crop-input" type="file" v-model="form.img_url" accept="image/*" @change="setImage"/>
                        </div>
                    </div>

                    <el-dialog title="裁剪图片" :visible.sync="dialogVisible" width="30%">
                        <vue-cropper ref='cropper' :src="imgSrc" :ready="cropImage" :zoom="cropImage" :cropmove="cropImage" style="width:100%;height:300px;"></vue-cropper>
                        <span slot="footer" class="dialog-footer">
                    <el-button @click="cancelCrop">取 消</el-button>
                    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
                </span>
                    </el-dialog>-->



                    <el-form-item label="描述内容">
                        <el-input type="textarea" rows="5" v-model="form.description"></el-input>
                    </el-form-item>
                    <el-form-item label="详情">
                    </el-form-item>
                    <el-form-item class="container" style="width:200%">
                        <!--<div class="plugins-tips">
                            Vue-Quill-Editor：基于Quill、适用于Vue2的富文本编辑器。
                            访问地址：<a href="https://github.com/surmon-china/vue-quill-editor" target="_blank">vue-quill-editor</a>
                        </div>-->
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
    import ElSlPanel from "element-ui/packages/color-picker/src/components/sv-panel";
    import VueCropper  from 'vue-cropperjs';
    export default {
        name: 'baseform',
        data: function(){
            return {
                form: {
                    title: '',
                    cat_id: '',
                    img_url: '',
                    description: '',
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

                },
                cateList:[],
                defaultSrc: './static/img/img.jpg',
            }
        },
        components: {
            ElSlPanel,
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
                    this.cateList = res.data.list;
                })
            },
            onSubmit() {
                let url = domain.testUrl+'/articleAdd';

                this.$axios.post(url,{
                    title:this.form.title,
                    cat_id:this.form.cat_id,
                    img_url:this.form.img_url,
                    description:this.form.description,
                    content:this.form.content,
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

                console.log(this.form);
                this.$message.success('提交成功！');
            }
        }
    }
</script>

<style scoped>
    .content-title{
        font-weight: 400;
        line-height: 50px;
        margin: 10px 0;
        font-size: 22px;
        color: #1f2f3d;
    }
    .pre-img{
        width: 100px;
        height: 100px;
        background: #f8f8f8;
        border: 1px solid #eee;
        border-radius: 5px;
    }
    .crop-demo{
        display: flex;
        align-items: flex-end;
    }
    .crop-demo-btn{
        position: relative;
        width: 100px;
        height: 40px;
        line-height: 40px;
        padding: 0 20px;
        margin-left: 30px;
        background-color: #409eff;
        color: #fff;
        font-size: 14px;
        border-radius: 4px;
        box-sizing: border-box;
    }
    .crop-input{
        position: absolute;
        width: 100px;
        height: 40px;
        left: 0;
        top: 0;
        opacity: 0;
        cursor: pointer;
    }
</style>
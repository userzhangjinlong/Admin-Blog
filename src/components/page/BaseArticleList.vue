<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-cascades"></i> 文章列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
                <el-input v-model="keyword" placeholder="筛选关键词名称、分类名称" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="search" @click="search">搜索</el-button>
            </div>

            <el-table :data="data" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="created_at" label="日期" sortable width="300">
                </el-table-column>
                <el-table-column prop="title" label="文章标题" width="240">
                </el-table-column>
                <el-table-column prop="cat_name" label="文章分类" width="240">
                </el-table-column>
                <!--<el-table-column prop="address" label="地址" :formatter="formatter">
                </el-table-column>-->
                <el-table-column label="操作" width="360" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="el-icon-edit" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>

            <div class="pagination">
                <el-pagination background @current-change="handleCurrentChange" layout="prev, pager, next" :total="totalData">
                </el-pagination>
            </div>
        </div>

        <!-- 编辑弹出框 -->
        <el-dialog title="编辑" :visible.sync="editVisible" width="30%">
            <el-form ref="form" :model="form" label-width="50px">
                <el-form-item label="名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>

        <!-- 删除提示框 -->
        <el-dialog title="提示" :visible.sync="delVisible" width="300px" center>
            <div class="del-dialog-cnt">删除不可恢复，是否确定删除？</div>
            <span slot="footer" class="dialog-footer">
                <el-button @click="delVisible = false">取 消</el-button>
                <el-button type="primary" @click="deleteRow">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name: 'basetable',
        data() {
            return {
                url: domain.testUrl+'/articleList',
                tableData: [],
                cur_page: 1,
                multipleSelection: [],
                keyword: '',
                totalData:10,
                del_list: [],
                is_search: false,
                editVisible: false,
                delVisible: false,
                form: {
                    title: '',
                    cat_name: '',

                },
                idx: -1,
                ids: 0
            }
        },
        created() {
            this.getData();
        },
        computed: {
            data() {

                return this.tableData.filter((d) => {
                    let is_del = false;
                    // console.log(d);
                    for (let i = 0; i < this.del_list.length; i++) {
                        if (d.title === this.del_list[i].title) {
                            is_del = true;
                            break;
                        }
                    }
                    if (!is_del) {
                        if ((d.title.indexOf(this.keyword) > -1)) {
                            d.cat_name = d.category.name;
                            d.cat_id = d.category.id;
                            return d;
                        }
                    }
                })
            }
        },
        methods: {
            // 分页导航
            handleCurrentChange(val) {
                this.cur_page = val;
                this.getData();
            },
            // 获取 easy-mock 的模拟数据
            getData() {
                // 开发环境使用 easy-mock 数据，正式环境使用 json 文件
                /*if (process.env.NODE_ENV === 'development') {
                    this.url = domain.testUrl+'/categoryList';
                };*/
                this.$axios.post(this.url, {
                    page: this.cur_page,
                    /*keyword:this.keyword,*/
                }).then((res) => {
                    this.tableData = res.data.list.data;
                    this.totalData = (res.data.list.last_page)*10;
                })
            },
            search() {
                this.is_search = true;
            },
            formatter(row, column) {
                return row.address;
            },
            filterTag(value, row) {
                return row.tag === value;
            },
            //编辑页面跳转
            handleEdit(index, row) {
                /*this.idx = index;
                this.ids = row.id;*/
                this.$router.push({path: '../../articleAdd', query: {id: row.id}});
                // window.location.href='/articleAdd/id/'+row.id;
                // this.$router.push('/BaseArticleAdd.vue');

                /*const item = this.tableData[index];*/
                /*this.form = {
                    name: item.name,
                    created_at:row.created_at
                }
                this.editVisible = true;*/
            },
            handleDelete(index, row) {
                this.idx = index;
                this.ids = row.id;
                this.delVisible = true;
            },
            //批量删除
            delAll() {
                const length = this.multipleSelection.length;
                let str = '';
                let id_arr = [];
                for (let i = 0; i < length; i++) {
                    id_arr.push(this.multipleSelection[i].id);
                    str += this.multipleSelection[i].title + ' ';
                }
                let url = domain.testUrl+'/articleDelAll';
                this.$axios.post(url, {
                    id:id_arr
                }).then((res) => {
                    if(res.data.code == 200){
                        this.$message.success('删除了' + str);
                        this.multipleSelection = [];
                        this.del_list = this.del_list.concat(this.multipleSelection);
                    }else{
                        //删除失败
                        this.$message.error('删除失败');
                        this.multipleSelection = [];
                    }
                })

            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            // 保存编辑
            saveEdit() {
                let url = domain.testUrl+'/categorySave';
                this.$axios.post(url, {
                    id:this.ids,
                    name:this.form.name
                }).then((res) => {
                    console.log(this.form);
                    if(res.data.code == 200){
                        this.$message.success(`修改第 ${this.idx+1} 行成功`);
                        this.$set(this.tableData, this.idx, this.form);
                        this.editVisible = false;
                    }else{
                        this.$message.error(`修改第 ${this.idx+1} 行失败`);
                        this.editVisible = false;
                    }
                })
                /*this.$set(this.tableData, this.idx, this.form);
                this.editVisible = false;
                this.$message.success(`修改第 ${this.idx+1} 行成功`);*/
            },
            // 确定删除
            deleteRow(){
                let url = domain.testUrl+'/articleDel';
                this.$axios.post(url, {
                    id:this.ids,
                }).then((res) => {
                    console.log(res);
                    if(res.data.code == 200){
                        this.tableData.splice(this.idx, 1);
                        this.$message.success('删除成功');
                        this.delVisible = false;
                    }else{
                        this.$message.error('删除失败');
                        this.delVisible = false;
                    }
                })
            }
        }
    }

</script>

<style scoped>
    .handle-box {
        margin-bottom: 20px;
    }

    .handle-select {
        width: 120px;
    }

    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .del-dialog-cnt{
        font-size: 16px;
        text-align: center
    }
    .table{
        width: 100%;
        font-size: 14px;
    }
    .red{
        color: #ff0000;
    }
</style>

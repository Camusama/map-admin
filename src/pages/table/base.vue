<template>
  <div class="panel">
    <panel-title :title="$route.meta.title">
      <el-button @click.stop="on_refresh" size="small">
        <i class="fa fa-refresh"></i>
      </el-button>
      <router-link :to="{name: 'tableAdd'}" tag="span">
        <el-button type="primary" icon="plus" size="small">添加数据</el-button>
      </router-link>
    </panel-title>
    <div class="panel-body">
      <div style="width: 30%;margin-bottom: 20px">
        <el-input placeholder="请输入内容" v-model="searchkey" class="input-with-select">
          <el-select v-model="searchid" slot="prepend" placeholder="请选择方式" style="width: 130px;">
            <el-option label="按用户名查询" value="username"></el-option>
            <el-option label="按姓名查询" value="name"></el-option>
            <el-option label="按岗位查询" value="job"></el-option>
          </el-select>
          <el-button slot="append" @click="submit_search"><i class="fa fa-search" aria-hidden="true"></i></el-button>
        </el-input>
      </div>
      <el-table
        :data="table_data"
        v-loading="load_data"
        element-loading-text="拼命加载中"
        border
        @selection-change="on_batch_select">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          prop="id"
          label="id"
          width="80"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="username"
          label="用户名"
          width="120"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="130"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="sex"
          label="性别"
          width="120"
        >
          <template scope="props">
            <span v-text="props.row.sex == 1 ? '男' : '女'"></span>
          </template>
        </el-table-column>
        <el-table-column
          prop="phone"
          label="联系方式"
          width="177px"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="email"
          label="E-MAIL"
          width="250px"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="job"
          label="岗位"
          width="400px"
        >
        </el-table-column>
        <el-table-column
          prop="admin"
          label="是否管理员"
          width="120">
          <template scope="props">
            <span v-text="props.row.sex == 1 ? '是' : '否'"></span>
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          width="180">
          <template scope="props">
            <router-link :to="{name: 'tableUpdate', params: {id: props.row.id}}" tag="span">
              <el-button type="info" size="small" icon="edit">修改</el-button>
            </router-link>
            <el-button type="danger" size="small" icon="delete" @click="delete_data(props.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <bottom-tool-bar>
        <el-button
          type="danger"
          icon="delete"
          size="small"
          :disabled="batch_select.length === 0"
          @click="on_batch_del"
          slot="handler">
          <span>批量删除</span>
        </el-button>
        <div slot="page">
          <el-pagination
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-size="10"
            layout="total, prev, pager, next"
            :total="total">
          </el-pagination>
        </div>
      </bottom-tool-bar>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle, bottomToolBar} from 'components'

  export default{
    data(){
      return {
        searchkey:"",
        searchid:"",
        table_data: null,
        //当前页码
        currentPage: 1,
        //数据总条目
        total: 0,
        //每页显示多少条数据
        length: 15,
        //请求时的loading效果
        load_data: true,
        //批量选择数组
        batch_select: []
      }
    },
    components: {
      panelTitle,
      bottomToolBar,
    },
    created(){
      this.get_table_data()
    },
    methods: {
      submit_search() {
        if (this.searchkey === ""){
          this.get_table_data()
        }else if(this.searchid === "username"){
          let temp=[]
          this.table_data.forEach((item) => {
            if(item.username){
              if(item.username.indexOf(this.searchkey)>=0){
                temp.push(item)
              }
            }
            this.table_data=temp
          })
        }else if(this.searchid === "name"){
          let temp=[]
          this.table_data.forEach((item) => {
            if(item.name){
              if(item.name.indexOf(this.searchkey)>=0){
                temp.push(item)
              }
            }
            this.table_data=temp
          })
        }else if (this.searchid === "job"){
          let temp=[]
          this.table_data.forEach((item) => {
            if(item.job){
              if(item.job.indexOf(this.searchkey)>=0){
                temp.push(item)
              }
            }
            this.table_data=temp
          })
        }
      },
      on_refresh(){
        this.get_table_data()
      },
      //获取数据
      // $fetch.api_table 等于api/index.js
      get_table_data(){
        this.load_data = true
        this.$fetch.api_table.list({
          page: this.currentPage,
          length: this.length
        })
          .then((res) => {
            console.log(res)
            this.table_data = res.data.result
            this.currentPage = res.data.page
            this.total = res.data.total
            this.load_data = false
          })
          .catch(() => {
            this.load_data = false
          })
      },
      //单个删除
      delete_data(item){
        this.$confirm('此操作将删除该数据, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
          .then(() => {
            this.load_data = true
            this.$fetch.api_table.del(item)
              .then(({msg}) => {
                this.get_table_data()
                this.$message.success(msg)
              })
              .catch(() => {
              })
          })
          .catch(() => {
          })
      },
      //页码选择
      handleCurrentChange(val) {
        this.currentPage = val
        this.get_table_data()
      },
      //批量选择
      on_batch_select(val){
        this.batch_select = val
      },
      //批量删除
      on_batch_del(){
        this.$confirm('此操作将批量删除选择数据, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
          .then(() => {
            this.load_data = true
            this.$fetch.api_table.batch_del(this.batch_select)
              .then(({msg}) => {
                this.get_table_data()
                this.$message.success(msg)
              })
              .catch(() => {
              })
          })
          .catch(() => {
          })
      }
    }
  }
</script>

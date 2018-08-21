<template>
  <div class="panel">
    <panel-title :title="$route.meta.title">
      <el-button @click.stop="on_refresh" size="small">
        <i class="fa fa-refresh"></i>
      </el-button>
      <router-link :to="{name: 'addOrgan'}" tag="span">
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
          width="80">
        </el-table-column>
        <el-table-column
          prop="organ_id"
          label="组织ID"
          width="200"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="organ_name"
          label="组织名称"
          width="790"
        >
        </el-table-column>
        <el-table-column
          prop="deptlist"
          label="组织部门"
          width="400"
        >
          <template slot-scope="scope">
            <div v-for='v in scope.row.deptlist'>
              <div style="margin-top:4px;display: flex;justify-content:space-between;border-bottom: 1px solid #dfe6ec;
              padding: 1px 3px;">
                {{v.dept_name}}
                <el-button plain  size="mini" icon="delete" @click="delete_dept(v.dept_id,scope.row.organ_id)"></el-button>
              </div>
            </div>
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          width="165">
          <template scope="props">
            <router-link :to="{name: 'saveOrgan', params: {organ_id: props.row.organ_id}}" tag="span">
              <el-button type="info" size="small" icon="edit">修改</el-button>
            </router-link>
            <el-button type="danger" size="small" icon="delete" @click="delete_organ(props.organ_id)">删除</el-button>
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
          <div style="width: 120px;transform: translate(-110px,26px);color:#48576a;font-size: 14px">
            每页显示
            <el-input ref="iplength" v-model="length" max="99" min="3" size="mini" @blur="lengthchange" style="width:40px;"></el-input>
            条
          </div>
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
  import axios from 'axios'

  export default{
    data(){
      return {
        searchkey:"",
        searchid:"",
        //当前页码
        currentPage: 1,
        //数据总条目
        total: 0,
        //每页显示多少条数据
        length: 15,
        //请求时的loading效果
        load_data: false,
        //批量选择数组
        batch_select: [],
        // table_data: null,
        table_data:[
          {
            organ_id:1,
            organ_name:'洪山区工商局',
            deptlist:[
              {
                dept_id:1,
                dept_name:'财政部'
              },
              {
                dept_id:2,
                dept_name:'组织部'
              },
              {
                dept_id:3,
                dept_name:'人事部'
              },
              {
                dept_id:4,
                dept_name:'项目部'
              },
              {
                dept_id:5,
                dept_name:'管理部'
              }
            ]
          },
          {
            organ_id:2,
            organ_name:'洪山区税务局',
            deptlist:[
              {
                dept_id:1,
                dept_name:'财政部'
              },
              {
                dept_id:2,
                dept_name:'组织部'
              },
              {
                dept_id:3,
                dept_name:'人事部'
              },
              {
                dept_id:4,
                dept_name:'项目部'
              },
              {
                dept_id:5,
                dept_name:'管理部'
              }
            ]
          },
          {
            organ_id:3,
            organ_name:'武汉市政府办公厅',
            deptlist:[
              {
                dept_id:1,
                dept_name:'财政部'
              },
              {
                dept_id:2,
                dept_name:'组织部'
              },
              {
                dept_id:3,
                dept_name:'人事部'
              },
              {
                dept_id:4,
                dept_name:'项目部'
              },
              {
                dept_id:5,
                dept_name:'管理部'
              }
            ]
          }
        ]
      }
    },
    components: {
      panelTitle,
      bottomToolBar,
    },
    created(){
      // this.get_table_data()
    },
    methods: {
      lengthchange(){
        var  val =this.$refs.iplength.value
        if(parseInt(this.$refs.iplength.value)){
          if(val>40){
            this.length=30
          }else if(val<3){
            this.length=3
          }else{
            this.length=val
          }
        }else{
          this.length=10
        }
        this.get_table_data()
      },
      submit_search() {
        if (this.searchkey === ""){
          this.get_table_data()
        }else if(this.searchid === "username"){
          let temp=[]
          this.table_data.forEach((item) => {
            console.log(item.username)
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
        // this.load_data = true
       // axios.get('/api/user/getList',{
       //   params:{
       //     page: this.currentPage,
       //     length: this.length
       //   }
       // }).then((res)=>{
       //     console.log(res)
       //     this.table_data=res.data.result
       //     this.page=res.data.page
       //     this.total = res.data.total
       //     setTimeout(1000)
       //     this.load_data = false
       //   })
       //  this.$fetch.api_table.list({
       //    page: this.currentPage,
       //    length: this.length
       //  })
       //    .then((res) => {
       //      console.log(res)
       //      this.table_data = res.data.result
       //      this.currentPage = res.data.page
       //      this.total = res.data.total
       //      this.load_data = false
       //    })
       //    .catch(() => {
       //      this.load_data = false
       //    })
      },
      delete_dept(deptid,organid){

      },
      delete_organ(organid){

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

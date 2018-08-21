<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body"
         v-loading="load_data"
         element-loading-text="拼命加载中">
      <el-row>
        <el-col :span="8">
          <el-form ref="form" :model="form" :rules="rules" label-width="100px">
            <el-form-item label="岗位名称:" prop="username">
              <el-input v-model="form.job_name" placeholder="请输入内容" style="width: 250px;"></el-input>
            </el-form-item>
            <el-form-item label="岗位id:" prop="name">
              <el-input v-model="form.job_id" placeholder="请输入内容" style="width: 250px;"></el-input>
            </el-form-item>
            <el-form-item label="组织：">
              <el-select v-model="organ_id" placeholder="请选择">
                <el-option
                  v-for="item in organlist"
                  :key="item.organ_id"
                  :label="item.organ_name"
                  :value="item.organ_id">
                </el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="部门：">
              <el-select v-model="dept_id"  placeholder="请选择">
                <el-option
                  v-for="item in deptlist"
                  :key="item.dept_id"
                  :label="item.dept_name"
                  :value="item.dept_id">
                </el-option>
              </el-select>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="on_submit_form" :loading="on_submit_loading">立即提交</el-button>
              <el-button @click="$router.back()">取消</el-button>
            </el-form-item>
          </el-form>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle} from 'components'
  import axios from 'axios'
  const url="/api/user"
  export default{
    data(){
      return {
        organ_id:null,
        dept_id:null,
        form: {
          job_name:null,
          job_id: null,
          organ_name: null,
          organ_id:null,
          dept_name:null,
          dept_id:null
        },
        route_id: this.$route.params.job_id,
        load_data: false,
        on_submit_loading: false,
        rules: {
          job_name: [{required: true, message: '岗位名不能为空', trigger: 'blur'}],
          job_id: [{required: true, message: '岗位名不能为空', trigger: 'blur'}],
          organ_name: [{required: true, message: '岗位名不能为空', trigger: 'blur'}],
          dept_name: [{required: true, message: '岗位名不能为空', trigger: 'blur'}],
        },
        deptlist:[
          {
            dept_id:1,
            dept_name:"财政部"
          },{
            dept_id:2,
            dept_name:"人事部"
          },{
            dept_id:3,
            dept_name:"项目部"
          }
        ],
        organlist:[
          {
            organ_id:1,
            organ_name:"武汉市工商局"
          },{
            organ_id:2,
            organ_name:"洪山区工商局"
          },{
            organ_id:3,
            organ_name:"洪山区教育局"
          }
        ]
      }
    },
    created(){
//      console.log(this.route_id)
      this.route_id && this.get_form_data()
    },
    methods: {
      //获取数据
      get_form_data(){
        this.load_data = true
        this.$fetch.api_table.get({
          id: this.route_id
        })
          .then(({data}) => {
            this.form = data
            this.load_data = false
          })
          .catch(() => {
            this.load_data = false
          })
      },
      //时间选择改变时
      on_change_birthday(val){
        this.$set(this.form, 'birthday', val)
      },
      //提交
      on_submit_form(){
        this.$refs.form.validate((valid) => {
          if (!valid) return false
          this.on_submit_loading = true
          axios.get(url, {
            params: {
              job_id: this.form.person_id,
              dept_id: this.form.dept_id,
              job_name: this.form.job_name
            }
          }).then((res) => {
            console.log(res)
            this.table_data = res.data.result
            this.page = res.data.page
            this.total = res.data.total
            setTimeout(1000)
            this.load_data = false
          })
        })
//        this.$refs.form.validate((valid) => {
//          if (!valid) return false
//          this.on_submit_loading = true
//          this.$fetch.api_table.save(this.form)
//            .then(({msg}) => {
//              this.$message.success(msg)
//              setTimeout(this.$router.back(), 500)
//            })
//            .catch(() => {
//              this.on_submit_loading = false
//            })
//        })
      },
    },
    components: {
      panelTitle
    }
  }
</script>

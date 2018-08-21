<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body"
         v-loading="load_data"
         element-loading-text="拼命加载中">
      <el-row>
        <el-col :span="8">
          <el-form ref="form" :model="form" :rules="rules" label-width="100px">
            <el-form-item label="用户名:" prop="username">
              <el-input v-model="form.username" placeholder="请输入内容" style="width: 250px;"></el-input>
            </el-form-item>
            <el-form-item label="姓名:" prop="realname">
              <el-input v-model="form.realname" placeholder="请输入内容" style="width: 250px;"></el-input>
            </el-form-item>
            <el-form-item label="密码:" prop="realname">
              <el-input v-model="form.password" placeholder="请输入内容" style="width: 250px;"></el-input>
            </el-form-item>
            <el-form-item label="性别:">
              <el-radio-group v-model="form.gender" prop="gender">
                <el-radio label="男">男</el-radio>
                <el-radio label="女">女</el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="联系方式:" prop="telephone">
              <el-input
                placeholder="请输入内容"
                v-model="form.telephone"
                style="width: 250px;">
              </el-input>
            </el-form-item>
            <el-form-item label="E-MAIL:" prop="email">
              <el-input
                placeholder="请输入内容"
                v-model="form.email"
                style="width: 250px;">
              </el-input>
            </el-form-item>
            <el-form-item label="是否管理员:" prop="admin">
              <el-radio-group v-model="form.isadmin">
                <el-radio :label="1">是</el-radio>
                <el-radio :label="0">否</el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="岗位:" prop="job_id">
              <el-input
                placeholder="请输入内容"
                :controls="false"
                v-model="form.job_id"
                style="width: 300px;"
              >
              </el-input>
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
            <el-form-item label="岗位：">
              <el-select v-model="job_id" placeholder="请选择">
                <el-option
                  v-for="item in joblist"
                  :key="item.job_id"
                  :label="item.job_name"
                  :value="item.job_id">
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

  export default{
    data(){
      return {
        organ_id:null,
        job_id:null,
        dept_id:null,
        form: {
          username:null,
          password:null,
          realname: null,
          gender: null,
          telephone: null,
          job_id: null,
          isadmin:null,
          email:null
        },
        route_id: this.$route.params.person_id,
        load_data: false,
        on_submit_loading: false,
        rules: {
          username: [{required: true, message: '用户名不能为空', trigger: 'blur'}],
          password: [{required: true, message: '密码不能为空', trigger: 'blur'}],
          realname: [{required: true, message: '姓名不能为空', trigger: 'blur'}],
          gender: [{required: true, message: '性别不能为空', trigger: 'blur'}],
          job_id: [{required: true, message: 'JOB_ID不能为空', trigger: 'blur'}],
          email: [{required: true, message: '邮件不能为空', trigger: 'blur'}],
          telephone: [{required: true, message: '电话号码不能为空', trigger: 'blur'}],
        },
        joblist:[
          {
            job_id:1,
            job_name:"职员"
          },
          {
            job_id:2,
            job_name:"主任"
          },
          {
            job_id:3,
            job_name:"经理"
          }
        ],
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
      this.organ_id=1
      this.route_id && this.get_form_data()
      console.log(this.route_id)
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
          this.$fetch.api_table.save(this.form)
            .then(({msg}) => {
              this.$message.success(msg)
              setTimeout(this.$router.back(), 500)
            })
            .catch(() => {
              this.on_submit_loading = false
            })
        })
      }
    },
    components: {
      panelTitle
    }
  }
</script>

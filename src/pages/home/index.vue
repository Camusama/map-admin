<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>公告</span>
          <el-button style="float: right;" @click.stop="on_refresh" size="small">
            <i class="fa fa-refresh"></i>
          </el-button>
        </div>
        <div v-for="o in 4" :key="o" class="text item">
          {{'列表内容 ' + o }}
        </div>
      </el-card>
      <el-card class="box-card" style="margin-top: 40px">
        <div slot="header" class="clearfix">
          <span>新闻</span>
          <el-button style="float: right;" @click.stop="on_refresh" size="small">
            <i class="fa fa-refresh"></i>
          </el-button>
        </div>
        <div v-for="o in 4" :key="o" class="text item">
          {{'列表内容 ' + o }}
        </div>
      </el-card>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle} from 'components'

  export default{
    data(){
      return {

      }
    },
    components: {
      panelTitle
    },
    methods:{
      on_refresh(){

      }
    }
  }
</script>

<style>
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 100%;
  }
</style>

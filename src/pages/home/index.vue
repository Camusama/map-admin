<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <i class="fa fa-newspaper-o fa-lg" aria-hidden="true" ></i>
          <span>公告</span>
          <el-button style="float: right;" @click.stop="on_refreshtips" size="small">
            <i class="fa fa-refresh"></i>
          </el-button>
        </div>
        <div v-for="o in 10" :key="o" class="text item">
          {{'公告内容 ' + o }}
        </div>
        <bottom-tool-bar>
          <div slot="page" class="pagination">
            <el-pagination
              @current-change="handletipChange"
              :current-page="tipcurrentPage"
              :page-size="10"
              layout="total, prev, pager, next"
              :total="tiptotal">
            </el-pagination>
          </div>
        </bottom-tool-bar>
      </el-card>

      <el-card class="box-card" style="margin-top: 40px">
        <div slot="header" class="clearfix">
          <i class="fa fa-newspaper-o fa-lg" aria-hidden="true" ></i>
          <span>新闻</span>
          <el-button style="float: right;" @click.stop="on_refreshnews" size="small">
            <i class="fa fa-refresh"></i>
          </el-button>
        </div>
        <div v-for="o in 10" :key="o" class="text item">

          {{'新闻内容 ' + o }}
        </div>
        <bottom-tool-bar>
          <div slot="page" class="pagination">
            <el-pagination
              @current-change="handlenewsChange"
              :current-page="newscurrentPage"
              :page-size="10"
              layout="total, prev, pager, next"
              :total="newstotal">
            </el-pagination>
          </div>
        </bottom-tool-bar>
      </el-card>

    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle, bottomToolBar} from 'components'
  import axios from 'axios'
  export default{
    data(){
      return {
        tipcurrentPage:1,
        newscurrentPage:1,
        tiptotal:15,
        newstotal:15,
        tiplength:10,
        newslength:10,
        newsList:[],
        tipList:[]
      }
    },
    components: {
      panelTitle,
      bottomToolBar
    },
    methods:{
      on_refreshtips(){

      },
      on_refreshnews(){

      },
      get_news(){

      },
      get_tips(){

      },
      handletipChange(val){
        this.tipcurrentPage=val
        this.get_tips()
      },
      handlenewsChange(val){
        this.newscurrentPage=val
        this.get_news()
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
  .pagination{

    margin: 0 auto;
  }
</style>

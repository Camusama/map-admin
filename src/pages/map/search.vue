<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body">
      <baidu-map class="map" center="武汉" :zoom="15" :scroll-wheel-zoom="true">
        <bm-city-list anchor="BMAP_ANCHOR_TOP_LEFT"></bm-city-list>
        <bm-scale anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-scale>
        <bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" :showAddressBar="true" :autoLocation="true"></bm-geolocation>
        <bm-local-search :keyword="keywordsearch" :auto-viewport="true" :location="Localsearch"></bm-local-search>
        <bm-context-menu>
          <bm-context-menu-item :callback="getPosition" text="收藏点"></bm-context-menu-item>
          <bm-context-menu-item :callback="showCollect" text="收藏列表"></bm-context-menu-item>
        </bm-context-menu>
        <bm-point-collection :points="points" shape="BMAP_POINT_SHAPE_RHOMBUS" :size="psize" color="red" size="BMAP_POINT_SIZE_SMALL" @click=""></bm-point-collection>
      </baidu-map>
      <div class="input-wrapper">
        <el-input
          class="input ip1"
          placeholder="请输入地点"
          v-model="keyword"
          clearable>
          <el-button slot="append" @click="handlekeyword">
            <i class="fa fa-search" aria-hidden="true"></i>
          </el-button>
        </el-input>
        <!--<el-input-->
          <!--class="input ip2"-->
          <!--placeholder="请输入城市"-->
          <!--v-model="Local"-->
          <!--clearable>-->
          <!--<el-button slot="append" @click="handlelocal">-->
            <!--<i class="fa fa-search" aria-hidden="true"></i>-->
          <!--</el-button>-->
        <!--</el-input>-->
      </div>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle} from 'components'
  export default{
    data(){
      return {
        psize:"BMAP_POINT_SIZE_HUGE",
        points: [],
        Localsearch:"",
        keywordsearch:"",
        keyword:"",
        Local:"",
        refresh:true
      }
    },
    components: {
      panelTitle
    },
    mounted() {
    },
    methods:{
      handlelocal(){
        this.Localsearch=this.Local
      },
      handlekeyword(){
        this.keywordsearch=this.keyword
      },
      getPosition(e){
        this.points.push({lng:e.point.lng,lat:e.point.lat})
      },
      showCollect(){

      }
    }
  }
</script>
<style scoped>
  .map {
    width: 100%;
    height: 600px;
  }
  .input-wrapper{
    height: 550px;
  }
  .ip1{
    float: left;
    width: 46%;
    max-width: 500px;
    margin-right: 20px;
    transform: translateY(-40px);
  }
  .ip2{
    width: 46%;
    max-width:500px;
    float: left;
    transform: translateY(-40px);
  }
</style>

<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body">
      <baidu-map class="map" center="武汉" :zoom="15" :scroll-wheel-zoom="true">
        <bm-city-list anchor="BMAP_ANCHOR_TOP_LEFT"></bm-city-list>
        <bm-scale anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-scale>
        <bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" :showAddressBar="true" :autoLocation="true"></bm-geolocation>
        <bm-local-search :keyword="keywordsearch" :auto-viewport="true" :location="Localsearch"></bm-local-search>
      </baidu-map>
      <div class="input-wrapper">
        <el-input
          class="input ip1"
          placeholder="请输入地点"
          v-model="keyword"
          clearable>
          <el-button slot="append">
            <i class="fa fa-search" aria-hidden="true"></i>
          </el-button>
        </el-input>
        <el-input
          class="input ip2"
          placeholder="请输入城市"
          v-model="Local"
          clearable>
          <el-button slot="append">
            <i class="fa fa-search" aria-hidden="true"></i>
          </el-button>
        </el-input>
      </div>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle} from 'components'
  export default{
    data(){
      return {
        Localsearch:"武汉",
        keywordsearch:"华中农业大学",
        keyword:"",
        Local:""
      }
    },
    components: {
      panelTitle
    },
    mounted() {
    },
    methods:{

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
    width: 500px;
    margin-right: 20px;
    transform: translateY(-40px);
  }
  .ip2{
    width: 500px;
    float: left;
    transform: translateY(-40px);
  }
</style>

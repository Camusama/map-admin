<template>
  <div class="panel">
    <panel-title :title="$route.meta.title"></panel-title>
    <div class="panel-body">
      <baidu-map class="bm-view" id="maphot" :center="location" :zoom="15" :scroll-wheel-zoom="true">
        <bm-scale anchor="BMAP_ANCHOR_BOTTOM_RIGHT"></bm-scale>
        <bm-map-type :map-types="['BMAP_NORMAL_MAP', 'BMAP_HYBRID_MAP']" anchor="RIGHT"></bm-map-type>
        <bm-geolocation anchor="BMAP_ANCHOR_TOP_LEFT" :showAddressBar="true" :autoLocation="true"></bm-geolocation>
      </baidu-map>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {panelTitle} from 'components'
  import { BaiduMap } from "vue-baidu-map";
  export default{
    data(){
      return {
        msg: 'home page',
        location:'武汉'
      }
    },
    components: {
      panelTitle
    },
    mounted() {
      // this.initMap();
    },
    methods:{
      // initMap: function() {
      //   var map = new BMap.Map("mymap");
      //   map.centerAndZoom("武汉", 15);
      //   map.enableScrollWheelZoom(true); //设置可缩放
      // }
    }
  }
</script>
<style>
  #maphot{
    widht:100%;
    height:600px;
  }
</style>

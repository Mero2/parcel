<template>
  <div>
    <v-header :seller = "seller"></v-header>
    <div class = "tab">
        <router-link class = "tab-item" to = '/goods'>商品</router-link>
        <router-link class = "tab-item" to = '/ratings'>评论</router-link>
        <router-link class = "tab-item" to = '/seller'>商家</router-link>
    </div>
    <keep-alive>
      <router-view :seller = "seller"></router-view>      
    </keep-alive>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  import {urlParse} from '../src/common/js/utill.js';
  const ERR_OK = 0;
  export default{
    data () {
      return {
        seller: {
          id: (() => {
            let querParam = urlParse();
            return querParam.id;
          })()
        }
      };
    },
    components: {
      'v-header': header
    },
    created () {
     this.$router.push('/goods'); // 页面加载时跳转
     this.$http.get('/api/seller?id='+ this.seller.id).then((response) => {
       response = response.body;
       if (response.errno === ERR_OK) {
        //  this.seller = response.data;
        this.seller = Object.assign({}, this.seller, response.data)
       };
     });
    }
  };
</script>

<style>
.tab{
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
}
.tab .tab-item{
  flex: 1;
  text-align: center;
  display: block;
  font-size: 14px;
  color:rgb(104, 178, 253);
  /* border-bottom: 1px solid black; */
}
.tab::after{
  display: block;
  position: absolute;
  top: 174px;
  height: 0;
  width: 100%;
  border-bottom: 1px solid rgba(15, 33, 39, 0.1);
  content: ' ';
}
.tab .router-link-active{
  color: rgb(240,20,20);
}
@media all and (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5){
  .tab::after{
    -webkit-transform: scaleY(0.7);
    transform: scaleY(0.7);
  }
}
@media all and (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2){
  .tab::after{
    -webkit-transform: scaleY(0.5);
    transform: scaleY(0.5);
    }
  }
</style>

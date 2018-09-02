<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar">
                <img width = "64" height = "64" :src="seller.avatar">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class = "support">
                    <span class="icon" :class = "classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="support-count" @click= "showDetail">
                <span class="count">{{seller.supports.length}}个 ></span>
            </div>
        </div>
        <div class="bulletin-wrapper" @click = "showDetail">
            <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
            <span class="iconde"> > </span>
        </div>
        <div class="background">
            <img :src="seller.avatar" width= "100%" height = "100%">
        </div>
        <div v-show = "detailShow" class="detail">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <star :size = "48" :score = "seller.score"></star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul v-if="seller.supports" class = "support">
                        <li class="support-item" v-for="(item, index) in seller.supports">
                            <span class="icon" :class = "classMap[seller.supports[index].type]"></span>
                            <span class="text">{{seller.supports[index].description}}</span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <div class="bulletin">
                        <p class="content">{{seller.bulletin}}</p>
                    </div>
                </div>
            </div>
            <div class="detail-close" @click="hideDetail">
                <span class="icon-close">X</span>
            </div>
        </div>
    </div>
</template>
<script>
import star from './../start/star.vue';
export default{
    props: {
        seller: {
            type: Object
        }
    },
    data () {
      return {
          detailShow: false
      };
    },
    methods: {
        showDetail () {
            this.detailShow = true;
        },
        hideDetail () {
            this.detailShow = false;
        }
    },
    created () {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
        star
    }
};
</script>
<style>
*{
    padding: 0;
    margin: 0;
}
body html{
    line-height: 1px;
    font-weight: 200px;
    font-family: 'PingFang SC','STHeitiSC-Light','Helvetica-Light','arial';
}
.header{
    color: #fff;
    background: rgba(7, 17, 27, 0.5);
    position: relative;
    overflow: hidden;
}
.header .content-wrapper{
    padding: 24px 12px 18px 24px;
    position: relative;
}
.header .content-wrapper .avatar{
    display: inline-block;
    vertical-align: top;
}
.header .content-wrapper .avatar img{
    border-radius: 2px;
}
.header .content-wrapper .content{
    display: inline-block;
    margin-left: 16px;
}
.header .content-wrapper .content .title{
    margin: 2px 0 8px 0;
}
.header .content-wrapper .content .title .brand{
    display: inline-block;
    width: 30px;
    height: 18px;
    vertical-align: top;
    background-size: 30px 18px;
    background-repeat: no-repeat;
}
@media all and (min-device-pixel-ratio: 3),(-webkit-min-device-pixel-ratio: 3) {
    .header .content-wrapper .content .title .brand{
        background-image: url("./brand@3x.png");
    }
    .header .content-wrapper .content .support .decrease{
        background-image: url("./decrease_1@3x.png");
    }
    .header .content-wrapper .content .support .discount{
        background-image: url("./discount_1@3x.png");
    }
    .header .content-wrapper .content .support .guarantee{
        background-image: url("./guarantee_1@3x.png");
    }
    .header .content-wrapper .content .support .invoice{
        background-image: url("./invoice_1@3x.png");
    }
    .header .content-wrapper .content .support .special{
        background-image: url("./special_1@3x.png");
    }
    .header .bulletin-wrapper .bulletin-title{
        background-image: url("./bulletin@3x.png");
    }
}
@media all and (min-device-pixel-ratio: 2),(-webkit-min-device-pixel-ratio: 2) {
    .header .content-wrapper .content .title .brand{
        background-image: url("./brand@2x.png")
    }
    .header .content-wrapper .content .support .decrease{
        background-image: url("./decrease_1@2x.png");
    }
    .header .content-wrapper .content .support .discount{
        background-image: url("./discount_1@2x.png");
    }
    .header .content-wrapper .content .support .guarantee{
        background-image: url("./guarantee_1@2x.png");
    }
    .header .content-wrapper .content .support .invoice{
        background-image: url("./invoice_1@2x.png");
    }
    .header .content-wrapper .content .support .special{
        background-image: url("./special_1@2x.png");
    }
    .header .bulletin-wrapper .bulletin-title{
        background-image: url("./bulletin@2x.png");
    }
}
.header .content-wrapper .content .title .name{
    margin-left: 6px;
    font-size: 16px;
    line-height: 18px;
    font-weight: bold;
}
.header .content-wrapper .content .description{
    margin-bottom: 10px;
    line-height: 12px;
    font-size: 12px;
}
.header .content-wrapper .content .support .icon{
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    vertical-align: top;
    background-size: 12px 12px;
    background-repeat: no-repeat;
}
.header .content-wrapper .content .support .text{
    line-height: 12px;
    font-size: 10px;   
    vertical-align: top;
}
.header .content-wrapper .support-count{
    position: absolute;
    right: 12px;
    bottom: 14px;
    padding: 0 8px;
    height: 24px;
    line-height: 24px;
    border-radius: 14px;
    background: rgba(0, 0, 0, 0.2);
    text-align: center; 
}
.header .content-wrapper .support-count .count{
    vertical-align: top;
    font-size: 10px;
}
.header .content-wrapper .support-count .count{
    font-size: 10px;
    vertical-align: top;
}
.header .bulletin-wrapper{
    position: relative;
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background: rgba(7, 17, 27, 0.2);
}
.header .bulletin-wrapper .bulletin-title{
    display: inline-block;
    width: 22px;
    height: 12px;
    vertical-align: top;
    margin-top: 8px;
    background-size: 22px 12px;
    background-repeat: no-repeat;
}
.header .bulletin-wrapper .bulletin-text{
    font-size: 10px;
    margin: 0 4px;
    vertical-align: top;
}
.header .bulletin-wrapper .iconde{
    display: inline-block;
    position: absolute;
    color: #fff;
    /* top: 8px; */
    right: 8px;
    width: 12px;
    height: 12px;
    /* margin-top: 7px; */
    vertical-align: top;
    font-size: 10px;
}
.header .background{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px)
}
.header .detail{
    position: fixed;
    z-index: 100;
    top: 0;
    right: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    transition: all 0.5s;
    background: rgba(7, 17, 27, 0.8);
    -webkit-backdrop-filter: blur(10px);
}
.clearfix{
    display: inline-block;   
}
.clearfix::after{
    display: block;
    content: ".";
    height: 0;
    line-height: 0;
    clear: both;
    visibility: hidden;
}
.header .detail .detail-wrapper{
    min-height: 100%;
    width: 100%;
}
.header .detail .detail-wrapper .detail-main{
    margin-top: 64px;
    padding-bottom: 64px;
}
.header .detail .detail-close{
    position: relative;
    width: 100%;
    height: 32px;
    margin: -64px auto 0;
    clear: both;
    font-size: 32px;
}
.header .detail .detail-close .icon-close{
    position: absolute;
    width: 32px;
    height: 32px;
    left: 50%;
    margin-left: -16px;
    line-height: 32px;
}
.header .detail .detail-wrapper .detail-main .name{
    line-height: 16px;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
}
.header .detail .detail-wrapper .detail-main .star-wrapper{
    margin-top: 18px;
    padding: 2px 0;
    text-align: center;
}
.header .detail .detail-wrapper .detail-main .title{
    display: flex;
    width: 80%;
    margin: 28px auto 24px;
}
.header .detail .detail-wrapper .detail-main .title .line{
    flex: 1;
    position: relative;
    top: -6px;
    border-bottom: 1px solid rgba(225, 225, 225, 0.2);
}
.header .detail .detail-wrapper .detail-main .title .text{
    padding: 0 12px;
    font-size: 14px;
    font-weight: 700;
}
.header .detail .detail-wrapper .detail-main .support{
    width: 80%;
    margin: 0 auto;
}
.header .detail .detail-wrapper .detail-main .support .support-item{
    padding: 0 12px;
    margin-bottom: 12px;
    font-size: 0;
}
.header .detail .detail-wrapper .detail-main .support .support-item:last-child{
    margin-bottom: 0;   
}
.header .detail .detail-wrapper .detail-main .support .support-item .icon{
    display: inline-block;
    width: 16px;
    height: 16px;
    vertical-align: top;
    margin-right: 6px;
    background-size: 16px 16px;
    background-repeat: no-repeat;
}
@media all and (min-device-pixel-ratio: 3),(-webkit-min-device-pixel-ratio: 3) {
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.decrease{
        background-image: url("./decrease_2@3x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.discount{
        background-image: url("./discount_2@3x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.guarantee{
        background-image: url("./guarantee_2@3x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.invoice{
        background-image: url("./invoice_2@3x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.special{
        background-image: url("./special_2@3x.png");
    }
}
@media all and (min-device-pixel-ratio: 2),(-webkit-min-device-pixel-ratio: 2) {
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.decrease{
        background-image: url("./decrease_2@2x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.discount{
        background-image: url("./discount_2@2x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.guarantee{
        background-image: url("./guarantee_2@2x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.invoice{
        background-image: url("./invoice_2@2x.png");
    }
    .header .detail .detail-wrapper .detail-main .support .support-item .icon.special{
        background-image: url("./special_2@2x.png");
    }
}
.header .detail .detail-wrapper .detail-main .support .support-item .text{
    line-height: 12px;
    font-size: 12px;
}
.header .detail .detail-wrapper .detail-main .bulletin{
    width: 80%;
    margin: 0 auto;
}
.header .detail .detail-wrapper .detail-main .content{
    padding: 0 12px;
    line-height: 24px;
    font-size: 12px;
}
</style>
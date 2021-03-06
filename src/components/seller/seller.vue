<template>
    <div class="seller" ref = "seller">
        <div class="seller-content">
            <div class="overview">
                <h1 class="title">{{seller.name}}</h1>
                <div class="desc">
                    <star :size = "36" :scroll = "seller.score"></star>
                    <span class="text">({{seller.ratingCount}})</span>
                    <span class="text">月售{{seller.sellCount}}单</span>
                </div>
                <ul class="remark">
                    <li class="block">
                        <h2>起送价</h2>
                        <div class="content">
                            <span class="stress">{{seller.minPrice}}</span>元
                        </div>
                    </li>
                     <li class="block">
                        <h2>商家配送</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryPrice}}</span>元
                        </div>
                    </li>
                     <li class="block">
                        <h2>平均配送时间</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryTime}}</span>分钟
                        </div>
                    </li>
                </ul>
                <div class="favorite" @click = "toggleFavorite">
                    <span class="icon" :class = "{'active': favorite}"></span>
                    <span class="text">{{favoriteText}}</span>
                </div>
            </div>
            <split></split>
            <div class="bulletin">
                <h1 class="title">公告与活动</h1>
                <div class="content-wrapper">
                    <p class="content">{{seller.bulletin}}</p>
                </div>
                <ul v-if="seller.supports" class = "support">
                    <li class="support-item" v-for="(item, index) in seller.supports">
                        <span class="icon" :class = "classMap[seller.supports[index].type]"></span>
                        <span class="text">{{seller.supports[index].description}}</span>
                    </li>
                </ul>
            </div>
            <split></split>
            <div class="pics">
                <h1 class="title">商家实景</h1>
                <div class="pic-wrapper" ref = "picWrapper">
                    <ul class="pic-list" ref = "picList">
                        <li class="pic-item" v-for = "pic in seller.pics">
                            <img :src="pic" width="120" height="90">
                        </li>
                    </ul>
                </div>
            </div>
            <split></split>
            <div class="info">
                <h1 class="title">商家信息</h1>
                <ul>
                    <li class="info-item" v-for = "info in seller.infos">{{info}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
import star from '../start/star.vue';
import BScroll from 'better-scroll';
import split from '../split/split.vue';
import {saveToLocal, loadFromLocal} from '../../common/js/store.js';
    export default {
        data() {
            return {
                favorite: (() => {
                    return loadFromLocal(this.seller.id,'favorite', false)
                })()
            }
        },
        props: {
            seller: {
                type: Object
            }
        },
        computed: {
            favoriteText() {
                return this.favorite ? '已收藏' : '收藏';
            }
        },
        components: {
            star,
            split
        },
        created () {
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        },
        mounted () {
            this.scroll = new BScroll(this.$refs.seller, {
                click: true
            })
            this._initPics();
        },
        watch: {
            'seller'() {
                this._initScroll();
                this._initPics();
            }
        },
        methods: {
            _initScroll() {
                if(!this.scroll) {
                    this.scroll = new BScroll(this.$refs.seller, {
                        click: true
                    });
                }else{
                    this.scroll.refresh();
                }
            },
            _initPics() {
                if(this.seller.pics) {
                    let picWidth = 120;
                    let margin = 6;
                    let width = (picWidth + margin) *this.seller.pics.length - margin;
                    this.$refs.picList.style.width = width + 'px';
                    this.$nextTick(() => {
                        if(!this.picScroll) {
                            this.picScroll = new BScroll(this.$refs.picWrapper, {
                            scrollX:true,
                            eventPassthrough: 'vertical'
                            })
                        }else{
                            this.scroll.refresh();
                        }
                    })
                }
            },
            toggleFavorite(event) {
                if(!event._constructed) {
                    return;
                }
                this.favorite = !this.favorite;
                saveToLocal(this.seller.id, 'favorite', this.favorite);  
            }
        }
    };
</script>
<style>
.seller{
    position: absolute;
    top: 176px;
    bottom: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
}
.seller .overview{
    position: relative;
    padding: 18px;
}
.seller .overview .title {
    margin-bottom: 8px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
}
.seller .overview .desc{
    padding-bottom: 18px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    font-size: 0;
}
.seller .overview .desc .star{
    display: inline-block;
    margin-right: 8px;
    vertical-align: top;
}
.seller .overview .desc .text{
    display: inline-block;
    margin-right: 12px;
    vertical-align: top;
    font-size: 10px;
    line-height: 18px;
    color: rgb(77, 85, 93);
}
.seller .overview .remark{
    display: flex;
    padding-top: 18px;
}
.seller .overview .remark .block{
    flex: 1;
    text-align: center;
    border-right: 1px solid rgba(7, 17, 27, 0.1);
}
.seller .overview .remark:last-child{
    border: none;
}
.seller .overview .remark .block h2{
    margin-bottom: 4px;
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
}
.seller .overview .remark .block .content{
    line-height: 24px;
    font-size: 10px;
    color: rgb(7, 17, 27);
}
.seller .overview .remark .block .content .stress{
    font-size: 24px;
}
.seller .bulletin{
    padding: 18px 18px 0 18px;
}
.seller .bulletin .title{
    margin-bottom: 8px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7, 17, 27);
}
.seller .bulletin .content-wrapper{
    padding: 0 12px 16px 12px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.seller .bulletin .content-wrapper .content{
    line-height: 24px;
    font-size: 12px;
    color: rgb(240, 20, 20);
}
.seller .bulletin .support .support-item{
    padding: 16px 12px;
    font-size: 0;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.seller .bulletin .support .support-item:last-child{
    border: none;
}
.seller .bulletin .support .support-item .icon{
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: 6px;
    vertical-align: top;
    background-size: 16px 16px;
    background-repeat: no-repeat;
}
.seller .bulletin .support .support-item .text{
    font-size: 12px;
    line-height: 16px;
    color: rgb(7, 17, 27);
}
@media all and (min-device-pixel-ratio: 3),(-webkit-min-device-pixel-ratio: 3) {
    .seller .bulletin .support .support-item .icon.decrease{
        background-image: url("./decrease_4@3x.png");
    }
    .seller .bulletin .support .support-item .icon.discount{
        background-image: url("./discount_4@3x.png");
    }
    .seller .bulletin .support .support-item .icon.guarantee{
        background-image: url("./guarantee_4@3x.png");
    }
    .seller .bulletin .support .support-item .icon.invoice{
        background-image: url("./invoice_4@3x.png");
    }
    .seller .bulletin .support .support-item .icon.special{
        background-image: url("./special_4@3x.png");
    }
}
@media all and (min-device-pixel-ratio: 2),(-webkit-min-device-pixel-ratio: 2) {
    .seller .bulletin .support .support-item .icon.decrease{
        background-image: url("./decrease_4@2x.png");
    }
    .seller .bulletin .support .support-item .icon.discount{
        background-image: url("./discount_4@2x.png");
    }
    .seller .bulletin .support .support-item .icon.guarantee{
        background-image: url("./guarantee_4@2x.png");
    }
    .seller .bulletin .support .support-item .icon.invoice{
        background-image: url("./invoice_4@2x.png");
    }
    .seller .bulletin .support .support-item .icon.special{
        background-image: url("./special_4@2x.png");
    }
}
.seller .pics{
    padding: 18px;
}
.seller .pics .title{
    margin-bottom: 12px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
}
.seller .pics .pic-wrapper{
    width: 100%;
    overflow: hidden;
    white-space: nowrap;
}
.seller .pics .pic-wrapper .pic-list{
    font-size: 0;
}
.seller .pics .pic-wrapper .pic-list .pic-item{
    display: inline-block;
    margin-right: 6px;
    width: 120px;
    height: 90px;
}
.seller .pics .pic-wrapper .pic-list:last-child{
    margin: 0;
}
.seller .info{
    padding: 18px 18px 0 18px;
}
.seller .info .title{
    padding-bottom: 12px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.seller .info .info-item{
    padding: 16px 12px;
    line-height: 16px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    font-size: 12px;
}
.seller .info .info-item:last-child{
    border: none;
}
.seller .overview .favorite{
    position: absolute;
    right: 18px;
    top: 18px;
    text-align: center;
}
.seller .overview .favorite .icon{
    width: 20px;
    height: 20px;
    background-size: 100%;
    display: block;
    background-image: url("./shoucang.png");  
}
.seller .overview .favorite .icon.active{
    margin-left: 5px;
    background-image: url("./yishoucang.png");
}
.seller .overview .favorite .text{
    line-height: 10px;
    font-size: 10px;
    color: rgb(77, 85, 93);
}
</style>

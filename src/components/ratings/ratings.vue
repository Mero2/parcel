<template>
    <div class="ratings" ref = "ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title1">服务态度</span>
                        <star :size = "36" :score = "seller.serviceScore"></star>
                        <span class="score1">{{seller.serviceScore}}</span>
                    </div>
                    <div class="score-wrapper">
                        <span class="title1">商品评分</span>
                        <star :size = "36" :score = "seller.foodScore"></star>
                        <span class="score1">{{seller.foodScore}}</span>
                    </div>
                    <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery-time">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
            <ratingselect @select = "selectRating" @toggle = "toggleContent" :select-type = "selectType" :only-content = "onlyContent" :ratings = "ratings"></ratingselect>
            <div class="rating-wrapper">
                <ul>
                    <li v-show="needShow(rating.rateType, rating.text)" v-for = "rating in ratings" class = "rating-item">
                        <div class="avatar">
                            <img width ="28" height ="28" :src="rating.avatar">
                        </div>
                        <div class="content">
                            <h1 class="name">{{rating.username}}</h1>
                            <div class="star-wrapper">
                                <star :size = "24" :score = "rating.score"></star>
                                <span class="delivery" v-show = "rating.deliveryTime">{{rating.deliveryTime}}分钟</span>
                            </div>
                            <p class="text">{{rating.text}}</p>
                            <div class="recommend" v-show = "rating.recommend && rating.recommend.length" >
                                <span class = "item" v-for = "(item, index) in rating.recommend" :key = "index">{{item}}</span>
                            </div>
                            <div class="time">{{rating.rateTime | formatDate}}</div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
import star from '../start/star.vue';
import split from '../split/split.vue';
import BScroll from 'better-scroll';
import {formatDate} from '../../common/js/data.js';
import ratingselect from '../ratingselect/ratingselect.vue';
const ALL = 2;
const ERR_OK = 0;
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                ratings: [],
                showFlag: false,
                selectType: ALL,
                onlyContent: true
            };
        },
        methods: {
            needShow (type, text) {
                if (this.onlyContent && !text) {
                return false
                }
                if (this.selectType === ALL) {
                return true
                } else {
                return type === this.selectType
                }
            },
            selectRating (type) {
                this.selectType = type
                this.$nextTick(() => {
                this.scroll.refresh()
                })
            },
            toggleContent () {
                this.onlyContent = !this.onlyContent
                this.$nextTick(() => {
                this.scroll.refresh()
                })
            }
        },
        created() {
            this.$http.get('/api/ratings').then((response) => {
                response = response.body;
                if(response.errno === ERR_OK) {
                    this.ratings = response.data;
                    this.$nextTick(() => {
                        this.scroll = new BScroll(this.$refs.ratings, {
                        click: true
                        })
                    })
                }
            });
        },
        components: {
            star,
            split,
            ratingselect
        },
         filters: {
            formatDate(time) {
                let date = new Date(time);
                return formatDate(date, 'yyyy-MM-dd hh:mm');
            }
        }
    };
</script>
<style>
.ratings{
    position: absolute;
    top: 174px;
    bottom: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
}
.ratings .overview{
    display: flex;
    padding: 18px 0;
}
.ratings .overview .overview-left{
    flex: 0 0 137px;
    padding: 6px 0;
    width: 137px;
    text-align: center;
    border-right: 1px solid rgba(7, 17, 27, 0.2);
}
.ratings .overview .overview-right{
    flex: 1;
    padding: 6px 0 6px 24px;
}
.ratings .overview .overview-left .score{
    margin-bottom: 6px;
    line-height: 28px;
    font-size: 24px;
    color: rgb(255, 153, 0);
}
.ratings .overview .overview-left .title{
    line-height: 12px;
    font-size: 12px;
    color: rgb(7, 17, 27);
}
.ratings .overview .overview-left .rank{
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
}
.ratings .ratings-content .overview .overview-right .score-wrapper{
    margin-bottom: 8px;
    font-size: 0;
}
.ratings .ratings-content .overview .overview-right .score-wrapper .title1{
    display: inline-block;
    font-size: 12px;
    line-height: 18px;
    color: rgb(7, 17, 27);
}
.ratings .ratings-content .overview .overview-right .score-wrapper .star{
    display: inline-block;
    margin: 0 12px;
    vertical-align: top;
}
.ratings .ratings-content .overview .overview-right .score-wrapper .score1{
    display: inline-block;
    vertical-align: top;
    line-height: 18px;
    font-size: 12px;
    color: rgb(255, 153, 0);
}
.ratings .ratings-content .overview .overview-right .delivery-wrapper{
    font-size: 0;
}
.ratings .ratings-content .overview .overview-right .delivery-wrapper .title{
    display: inline-block;
    font-size: 12px;
    line-height: 18px;
    color: rgb(7, 17, 27);
}
.ratings .ratings-content .overview .overview-right .delivery-wrapper .delivery-time{
    margin-left: 12px;
    font-size: 12px;
    color: rgb(147, 153, 159);
}
.ratings .ratings-content .rating-wrapper{
    padding: 0 18px;
}
.ratings .ratings-content .rating-wrapper .rating-item{
    display: flex;
    padding: 18px 0;
    border-bottom: 1px solid rgba(7, 17, 27, 0.2);
}
.ratings .ratings-content .rating-wrapper .rating-item .avatar{
    flex: 0 0 28px;
    width: 28px;
    margin-right: 12px;  
}
.ratings .ratings-content .rating-wrapper .rating-item .avatar img{
    border-radius: 50%;
}
.ratings .ratings-content .rating-wrapper .rating-item .content{
    position: relative;
    flex: 1;
}
.ratings .ratings-content .rating-wrapper .rating-item .content .name{
    margin-bottom: 4px;
    line-height: 12px;
    font-size: 10px;
    color: rgb(7, 17, 27);
}
.ratings .ratings-content .rating-wrapper .rating-item .star-wrapper{
    margin-bottom: 6px;
    font-size: 0;
}
.ratings .ratings-content .rating-wrapper .rating-item .star-wrapper .star{
    margin-right: 16px;
    display: inline-block;
    vertical-align: top;
}
.ratings .ratings-content .rating-wrapper .rating-item .star-wrapper .delivery{
    display: inline-block;
    vertical-align: top;
    color: rgb(147, 153, 159);
    line-height: 12px;
    font-size: 10px;
}
.ratings .ratings-content .rating-wrapper .rating-item .text{
    line-height: 18px;
    color: rgb(7, 17, 27);
    font-size: 12px;
    margin-bottom: 8px;
}
.ratings .ratings-content .rating-wrapper .rating-item .recommend{
    line-height: 16px;
    font-size: 0;
}
.ratings .ratings-content .rating-wrapper .rating-item .recommend .item{
    display: inline-block;
    margin: 0 8px 4px 0;
    font-size: 9px;
    padding: 0 6px;
    border: 1px solid rgba(7, 17, 27, 0.1);
    border-radius: 2px;
    color: rgb(147, 153, 159);
    background-color: #fff;
}
.ratings .ratings-content .rating-wrapper .rating-item .time{
    position: absolute;
    top: 0;
    right: 0;
    line-height: 12px;
    font-size: 10px;
    color: rgb(147, 153, 159);
}
</style>










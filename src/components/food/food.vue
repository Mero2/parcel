<template>
    <div class="food" v-show = "showFlag" ref = "food">
        <div class="food-content">
            <div class="image-header">
                <img :src="food.image">
                <div class="back" @click = "hide"><返回</div>
            </div>
            <div class="content">
                <h1 class="title">{{food.name}}</h1>
                <div class="detail">
                    <span class="sell-count">月售{{food.sellCount}}</span>
                    <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                    <span class="now">￥{{food.price}}</span><span class="old" v-show = "food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                <cartcontrol :food = "food"></cartcontrol>
                </div>
                <div @click.stop.prevent = "addFirst" class="buy" v-show = "!food.count || food.count === 0">加入购物车</div>
            </div>
            <split v-show = "food.info"></split>
            <div class="info" v-show = "food.info">
                <h1 class="title">商品信息</h1>
                <p class="text">{{food.info}}</p>
            </div>
            <split></split>
            <div class="rating">
                <h1 class="title">商品评价</h1>
                <ratingselect @select = "selectRating" @toggle = "toggleContent" :select-type = "selectType" :only-content = "onlyContent" :desc = "desc" :ratings = "food.ratings"></ratingselect>
                <div class="rating-wrapper">
                    <ul v-show = "food.ratings && food.ratings.length">
                        <li v-show = "needShow(rating.rateType, rating.text)" class="rating-item" v-for = "rating in food.ratings">
                            <div class="user">
                                <span class="name">{{rating.username}}</span>
                                <img width = "12" height = "12" :src="rating.avatar" class="avatar">
                            </div>
                            <div class="time">{{rating.rateTime | formatDate}}</div>
                            <p class="text">{{rating.text}}</p>
                        </li>
                    </ul>
                    <div class="no-rating" v-show = "!food.ratings || !food.ratings.length">暂无评价</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Vue from 'vue';
import {formatDate} from '../../common/js/data.js';
import BScroll from 'better-scroll';
import split from '../split/split.vue';
import ratingselect from '../ratingselect/ratingselect.vue';
import cartcontrol from '../cartcontrol/cartcontrol.vue';
const ALL = 2;
    export default{
        props: {
            food: {
                type: Object
            }
        },
        data() {
            return {
                showFlag: false,
                selectType: ALL,
                onlyContent: true,
                desc: {
                    all: '全部',
                    positive: '推荐',
                    negative: '吐槽'
                }
            }
        },
        methods: {
            show() {
                this.showFlag = true;
                this.selectType = ALL;
                this.onlyContent = true;
                this.$nextTick(() => {
                    if(!this.scroll) {
                        this.scroll = new BScroll(this.$refs.food, {
                            click: true
                        });
                    }else{
                        this.scroll.refresh();
                    }
                })
            },
            hide() {
                this.showFlag = false;
            },
            addFirst(event) {
                if(!event._constructed) {
                    return;
                }
                Vue.set(this.food, 'count', 1);
            },
            needShow(type, text) {
                if(this.onlyContent && !text) {
                    return false;
                }
                if(this.selectType === ALL) {
                    return true;
                }else{
                    return type === this.selectType;
                }
            },
            selectRating(type) {
                this.selectType = type;
                this.$nextTick(() => {
                    this.scroll.refresh();
                });
            },
            toggleContent(onlyContent) {
                this.onlyContent = onlyContent;
                this.$nextTick(() => {
                    this.scroll.refresh();
                });
            }
        },
        components: {
            cartcontrol,
            split,
            ratingselect
        },
        filters: {
            formatDate(time) {
                let date = new Date(time);
                return formatDate(date, 'yyyy-MM-dd hh:mm');
            }
        },
    };
</script>
<style>
.food{
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #fff;
}
.food .food-content .image-header{
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
}
.food .food-content .image-header img{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
.food .food-content .image-header .back{
    position: absolute;
    top: 10px;
    left: 10px;
    width: 30px;
    height: 15px;
    font-size: 10px;
    font-weight: 700;
    text-align: center;
    line-height: 15px;
    border-radius: 9px;
    border: 1px solid #fff;
    color: #fff;
}
.food .food-content .content{
    position: relative;
    padding: 18px;
}
.food .food-content .content .title{
    line-height: 14px;
    margin-bottom: 8px;
    font-size: 14px;
    font-weight: 700;
    color: rgb(7, 17, 27);
}
.food .food-content .content .detail{
    margin-bottom: 18px;
    line-height: 10px;
    font-size: 0;
    height: 10px;
}
.food .food-content .content .detail .sell-count,.rating{
    font-size: 10px;
    color: rgb(147, 153, 159);
}
.food .food-content .content .detail .sell-count{
    margin-right: 12px;
}
.food .food-content .content .price{
    font-weight: 700;
    line-height: 24px;
}
.food .food-content .content .price .now{
    margin-right: 18px;
    font-size: 16px;
    color: rgb(240, 20, 20);
}
.food .food-content .content .price .old{
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147, 153, 159);
}
.food .food-content .content .cartcontrol-wrapper{
    position: absolute;
    right: 20px;
    bottom: 20px;
}
.food .food-content .content .buy{
    position: absolute;
    right: 18px;
    bottom: 18px;
    z-index: 10;
    height: 24px;
    line-height: 24px;
    padding: 0 12px;
    box-sizing: border-box;
    border-radius: 12px;
    font-size: 10px;
    color: #fff;
    background: rgb(0, 160, 220);
}
.food .food-content .info{
    padding: 18px;
}
.food .food-content .info .title{
    line-height: 14px;
    margin-bottom: 6px;
    font-size: 16px;
    color: rgb(7, 17, 27);
}
.food .food-content .info .text{
    line-height: 24px;
    padding: 0 8px;
    font-size: 12px;
    color: rgb(77, 85, 93);
}
.food .food-content .rating{
    padding: 18px;
}
.food .food-content .rating .title{
    line-height: 14px;
    margin-left: 18px;
    font-size: 14px;
    color: rgb(7, 17, 27);
}
.food .food-content .rating .rating-wrapper{
    padding: 0 18px;
}
.food .food-content .rating .rating-wrapper .rating-item{
    position: relative;
    padding: 16px 0;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.food .food-content .rating .rating-wrapper .rating-item .user{
    position: absolute;
    right: 0;
    top: 16px;
    line-height: 12px;
    font-size: 0;
}
.food .food-content .rating .rating-wrapper .rating-item .user .name{
    display: inline-block;
    vertical-align: top;
    font-size: 10px;
    color: rgb(147, 153, 159);
}
.food .food-content .rating .rating-wrapper .rating-item .user .avatar{
    border-radius: 50%;
}
.food .food-content .rating .rating-wrapper .rating-item .time{
    margin-bottom: 6px;
    line-height: 12px;
    font-size: 10px;
    color: rgb(147, 153, 159);
}
.food .food-content .rating .rating-wrapper .rating-item .text{
    line-height: 16px;
    font-size: 12px;
    color: rgb(7, 17, 27);
}
.food .food-content .rating .rating-wrapper .no-rating{
    padding: 16px 0;
    font-size: 12px;
    color: rgb(147, 153, 159);
}
</style>


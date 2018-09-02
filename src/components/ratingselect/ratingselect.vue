<template>
    <div class="ratingselect">
        <div class="rating-type">
            <span @click = "select(2, $event)" class = "block positive" :class = "{'active': selectType === 2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
            <span @click = "select(0, $event)" class = "block positive" :class = "{'active': selectType === 0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
            <span @click = "select(1, $event)" class = "block negative" :class = "{'active': selectType === 1}">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
        </div>
        <div @click = "toggleContent" class="switch" :class = "{'on':onlyContent}">
            <span class="icon">√</span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>
<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
    export default{
        props: {
            ratings: {
                type: Array,
                default() {
                    return [];
                }
            },
            selectType: {
                type: Number,
                default: 2
            },
            onlyContent: {
                type: Boolean,
                default: false
            },
            desc: {
                type: Object,
                default() {
                    return{
                        all: '全部',
                        positive: '满意',
                        negative: '不满意'
                    }
                }
            }
        },
        computed: {
            positives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === POSITIVE;
                });
            },
            negatives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === NEGATIVE;
                });
            }
        },
        methods: {
            select(type, event) {
                if(!event._constructed) {
                    return;
                }
                this.$emit('select', type);
            },
            toggleContent(event) {
                if(!event._constructed) {
                    return;
                }
                this.$emit('toggle');
            }
        }
    };
</script>
<style>
.ratingselect .switch{
    padding: 12px 18px;
    line-height: 24px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    color: rgb(147, 153, 159);
}
.ratingselect .switch .icon{
    display: inline-block;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    line-height: 10px;
    text-align: center;
    border: 1px solid rgb(7, 17, 27);
    margin-right: 4px;
}
.ratingselect .switch .text{
    display: inline-block;
    font-size: 12px;
    vertical-align: top;
}
.ratingselect .switch.on .icon{
    background: #00c850;
    color: #fff;
}
.ratingselect .rating-type{
    padding: 18px 0;
    margin: 0 18px; 
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);  
}
.ratingselect .rating-type .block{
    display: inline-block;
    padding: 8px 12px; 
    margin-right: 8px;
    border-radius: 1px;
    color: rgb(77, 85, 93);
}
.ratingselect .rating-type .block.active{
    color: #fff;
}
.ratingselect .rating-type .block.positive{
    background: rgba(0, 160, 220, 0.2)
}
.ratingselect .rating-type .block.positive.active{
    background: rgb(0, 160, 220);
}
.ratingselect .rating-type .block.negative{
    background: rgba(77, 85, 93, 0.2);
}
.ratingselect .rating-type .block.negative.active{
    background: rgb(77, 85, 93);
}
.ratingselect .rating-type .block .count{
    margin-left: 2px;
    font-size: 8px;
    line-height: 16px;
}
</style>

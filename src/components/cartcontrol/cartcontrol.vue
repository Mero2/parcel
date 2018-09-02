<template>
    <div class="cartcontrol">
        <div class="cart-decrease" @click.stop.prevent = "decreaseCart" v-show = "food.count > 0">-</div>
        <div class="cart-count" v-show = "food.count > 0">{{food.count}}</div>
        <div class="cart-add" @click.stop.prevent = "addCart">+</div>
    </div>
</template>
<script>
import Vue from 'vue';
export default {
    props: {
        food: {
            type: Object
        }
    },
    methods: {
        addCart(event) {
            if(!event._constructed) {
                return;
            } 
            if(!this.food.count) {
                Vue.set(this.food, 'count', 1);
            }else{
                this.food.count ++;
            }
        },
        decreaseCart(event) {
            if(!event._constructed) {
                return;
            } 
            if(this.food.count) {
                this.food.count --;
            }
        }
    }
};
</script>
<style>
.cartcontrol{
    font-size: 0;
}
.cartcontrol .cart-decrease{
    display: inline-block;
    width: 12px;
    height: 12px;
    font-size: 12px;
    line-height: 12px;
    text-align: center;
    color: rgb(0, 160, 220);
    border: 1px solid rgb(0, 160, 220);
    border-radius: 50%;
}
.cartcontrol .cart-count{
    display: inline-block;
    padding: 0 6px;
    font-size: 10px;
    width: 12px;
    height: 12px;
    line-height: 24px;
    text-align: center;
    color: rgb(147, 153, 159);
}
.cartcontrol .cart-add{
    display: inline-block;
    width: 12px;
    height: 12px;
    font-size: 12px;
    line-height: 12px;
    text-align: center;
    color: rgb(0, 160, 220);
    border: 1px solid rgb(0, 160, 220);
    border-radius: 50%;
}
</style>


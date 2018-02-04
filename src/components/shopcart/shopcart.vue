<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'highLight': totalCount>0}">
                        <span class="icon-shopping_cart" :class="{'highLight': totalCount>0}"></span>
                    </div> 
                     <div class="num" v-show="totalCount>0">
                        {{totalCount}}
                    </div> 
                </div>
                 <div class="price" :class="{'highLight': totalPrice>0}">
                    <span>¥{{totalPrice}}</span>
                </div>
                <div class="desc">
                    <span>另需配送费¥{{deliveryPrice}}元</span>
                </div>
            </div>
            <div class="content-right">
                <div class="pay" :class="payClass">
                     {{payDesc}}
                </div>
               
            </div>
        </div>
    </div>
</template>
<script type="text/ecmascript-6">
    export default {
        props: {
            selectFoods: {
                type: Array,
                default() {
                    return [
                        {
                            price: 10,
                            count: 10
                        }
                    ];
                }
            },
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            }
        },
        computed: {
          totalPrice() {
              let total = 0;
              this.selectFoods.forEach(function(food) {
                  total += food.price * food.count;
              });
              return total;
          },
          totalCount() {
              let count = 0;
              this.selectFoods.forEach(function(food) {
                  count += food.count;
              });
              return count;
          },
          payDesc() {
              if (this.totalPrice === 0) {
                  return `还差${this.minPrice}元起送`;
              } else if (this.totalPrice < this.minPrice) {
                  let diff = this.minPrice - this.totalPrice;
                  return `还差${diff}元起送`;
              } else {
                  return '去结算';
              }
          },
          payClass() {
              if (this.totalPrice < this.minPrice) {
                  return 'not-enough';
              } else {
                  return 'enough';
              }
          }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .shopcart
        position: fixed 
        left: 0 
        bottom: -2px
        z-index: 50
        width: 100%
        height: 48px
        background: #000
        .content
            display: flex
            background: #141d27
            height: 100%
            color: rgba(255,255,255,0.4)
            .content-left
                flex: 1
                background: rgba(0,0,0,0.4)
                .logo-wrapper
                    display: inline-block
                    position: relative
                    vertical-align: top
                    margin: 0 12px
                    top: -10px
                    width: 56px
                    height: 56px
                    border-radius: 50%  
                    padding: 6px 
                    box-sizing: border-box 
                    background: #141d27   
                    .logo
                        width:100%
                        height: 100%
                        background: #2b343c
                        border-radius: 50%
                        text-align center
                        &.highLight
                            background: rgb(0,160,220)
                        .icon-shopping_cart
                            line-height: 44px
                            font-size: 24px
                            color: #80858a 
                            &.highLight 
                                color: #fff
                    .num
                        position: absolute
                        top: 0
                        right: 0
                        width: 24px
                        height: 16px
                        line-height: 16px
                        border-radius: 16px
                        text-align: center 
                        font-size: 9px
                        font-weight: 700
                        color: #fff
                        background: rgb(240,20,20) 
                        box-box-shadow: 0 4px 8px 0 gba(0,0,0,0.4)     
                .price
                    display: inline-block
                    vertical-align: top
                    line-height: 24px
                    padding-right: 12px
                    margin-top: 12px
                    box-sizing: border-box
                    border-right: 1px solid rgba(255,255,255,0.1)
                    font-size: 16px
                    font-weight: 700
                    &.highLight
                        color: #fff
                .desc
                    display: inline-block
                    vertical-align: top
                    margin: 12px 0 0 12px
                    line-height: 24px
                    font-weight: 700
                    font-size: 10px
            .content-right
                flex: 0 0 105px
                width: 105px
                .pay
                    height: 48px
                    line-height: 48px
                    font-size: 12px
                    font-weight: 700
                    font-size: 12px
                    text-align: center
                    &.not-enough
                        background: #2b333b
                    &.enough
                        background: #00b43c
                        color: #fff

</style>

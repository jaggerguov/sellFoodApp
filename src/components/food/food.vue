<template>
    <transition name='move'>
        <div v-show="showFlag" class="food" ref="food">
            <div class="image-header">
                <img :src="food.image" alt="">
            </div>
            <div class="back" @click="hide">
                <i class="icon-arrow_lift"></i>
            </div>
            <div class="content">
                <h1 class="title">{{food.name}}</h1>
                <div class="details">
                    <span class="sell-count">月售{{food.sellCount}}份</span>
                    <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                    <span class="newPrice">¥{{food.price}}</span><span v-show="food.oldPrice" class="oldPrice">¥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                    <cartcontrol  :food="food" @add="addFood"></cartcontrol>
                </div>
                <transition name="fade">
                    <div class="buy" v-show="!food.count || food.count===0"  @click.stop.prevent="addFirst">加入购物车</div>
                </transition>   
            </div>
            <split v-show="food.info"></split>
            <div class="info" v-show="food.info">
                <h1 class="title">商品信息</h1>
                <p class="text">{{food.info}}</p>
            </div>
            <split></split>
             <div class="rating">
                <h1 class="title">商品信息</h1>
                <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
                 <div class="rating-wrapper">
                    <ul v-show="food.ratings && food.ratings.length">
                        <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings"
                            class="rating-item border-1px">
                            <div class="user">
                            <span class="name">{{rating.username}}</span>
                            <img class="avatar" width="12" height="12" :src="rating.avatar">
                            </div>
                            <div class="time">{{rating.rateTime | formatDate}}</div>
                            <p class="text">
                            <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
                            </p>
                        </li>
                    </ul>
                    <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
                </div>
             </div>   
        </div>
    </transition>
</template>
<script type="text/ecmascript-6">
import BScroll from "better-scroll";
import Vue from "vue";
import { formatDate } from "../../common/js/date";
import cartcontrol from "../cartcontrol/cartcontrol";
import split from "../split/split";
import ratingselect from "../ratingselect/ratingselect";
const ALL = 2;
export default {
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
        all: "全部",
        positive: "推荐",
        negative: "吐槽"
      }
    };
  },
  methods: {
    show() {
      this.showFlag = true;
      this.selectType = ALL;
      this.onlyContent = true;
      if (!this.scroll) {
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          });
        });
      }
    },
    hide() {
      this.showFlag = false;
    },
    addFirst(event) {
      if (!event._constructed) {
        return;
      }
      this.$emit("add", event.target);
      Vue.set(this.food, "count", 1);
    },
    addFood(target) {
      this.$emit("add", target);
    },
    needShow(type, text) {
      if (this.onlyContent && !text) {
        return false;
      }
      if (this.selectType === ALL) {
        return true;
      } else {
        return type === this.selectType;
      }
    },
    selectRating(type) {
      this.selectType = type;
      this.$nextTick(() => {
        this.scroll.refresh();
      });
    },
    toggleContent() {
      this.onlyContent = !this.onlyContent;
      this.$nextTick(() => {
        this.scroll.refresh();
      });
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, "yyyy-MM-dd hh:mm");
      }
    }
  },
  components: {
    cartcontrol,
    split,
    ratingselect
  }
};
</script>
<style lang="stylus" rel="stylesheet/stylus" scoped>
@import '../../common/stylus/mixin.styl';

.food {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 50px;
    z-index: 30;
    width: 100%;
    background: #fff;
    overflow: auto;

    &.move-enter-active, &.move-leave-active {
        transition: all 0.2s linear;
    }

    &.move-enter, &.move-leave-active {
        transform: translate3d(100%, 0, 0);
    }

    .image-header {
        position: relative;
        top: 0;
        left: 0;
        height: 0;
        width: 100%;
        padding-top: 100%;

        img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
    }
}

.back {
    position: absolute;
    top: 20px;
    left: 0;

    .icon-arrow_lift {
        font-size: 16px;
        font-weight: bold;
        display: block;
        padding: 10px;
        color: #ddd;
    }
}

.content {
    position: relative;
    padding: 18px;

    .title {
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
        line-height: 14px;
        margin-bottom: 8px;
    }

    .details {
        font-size: 0;
        margin-bottom: 18px;

        .sell-count {
            margin-right: 12px;
        }

        .rating, .sell-count {
            font-size: 10px;
            color: rgb(147, 153, 159);
        }
    }

    .price {
        font-weight: 700;
        line-height: 24px;

        .newPrice {
            font-size: 14px;
            color: rgb(240, 20, 20);
            margin-right: 8px;
        }

        .oldPrice {
            text-decoration: line-through;
            font-size: 10px;
            color: rgb(147, 157, 159);
        }
    }

    .cartcontrol-wrapper {
        position: absolute;
        right: 12px;
        bottom: 12px;
    }

    .buy {
        position: absolute;
        bottom: 18px;
        right: 18px;
        width: 72px;
        height: 24px;
        border-radius: 12px;
        background: rgb(0, 160, 220);
        font-size: 10px;
        color: #ffffff;
        line-height: 24px;
        text-align: center;
        opacity: 1;

        &.fade-enter-active, &.fade-leave-active {
            transition: all 0.2s;
        }

        &.fade-enter, &.fade-leave-active {
            opacity: 0;
            z-index: -1;
        }
    }
}

.info {
    padding: 18px;

    .title {
        font-size: 14px;
        line-height: 14px;
        margin-bottom: 6px;
        color: rgb(7, 17, 27);
    }

    .text {
        font-size: 12px;
        line-height: 24px;
        font-weight: 200;
        color: rgb(77, 85, 93);
    }
}

.rating {
    padding-top: 18px;

    .title {
        font-size: 14px;
        line-height: 14px;
        margin-left: 18px;
        margin-bottom: 6px;
        color: rgb(7, 17, 27);
    }
}
</style>

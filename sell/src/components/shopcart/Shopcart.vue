<template>
    <div class="bannerWrapper">
        <div class="leftContent">
            <div class="cartLogo">
                <div class="logo" :class="{hasContent:getCount > 0}">
                    <i class="icon icon-shopping_cart"></i>
                </div>
            </div>
            <div class="num" v-show="getCount > 0">{{getCount}}</div>
            <div class="totalPrice">￥{{getTotalPrice}}元</div>
            <div class="disc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div class="rightContent" :class="{goal: isGotoDelivery === '去结算'}">
            <span class="pricStart">{{isGotoDelivery}}</span>
        </div>
        <transition-group name="drop" tag="div" class="ball-content"
            v-on:before-enter="beforeEnter"
            v-on:enter="dropEnter"
            v-on:after-enter="afterEnter"
        >
            <div class="ball" v-for="(ball,index) in balls" :key="index" v-show="ball.show">
                <div class="inner inner-hook"></div>
            </div>
        </transition-group>
    </div>
</template>

<script>
/* eslint space-before-function-paren: ["error", "never"] */
export default {
    data() {
        return {
            balls: [{
                show: true
            }, {
                show: false
            }, {
                show: false
            }, {
                show: false
            }, {
                show: false
            }
            ],
            dropBall: []
        }
    },
    methods: {
        _drop(el) {
            for (let i = 0; i < this.balls.length; i++) {
                let ball = this.balls[i]
                if (ball.show) {
                    ball.show = true
                    ball.el = el
                    this.dropBall.push(ball)
                    return
                }
            }
        },
        beforeEnter(el,done) {
            let count = this.balls.length
            while (count--) {
                let ball = this.balls[count]
                if (ball.show) {
                    let rect = ball.el.getBoundingClientRect()
                    let x = rect.left - 32
                    let y = -(window.innerHeight - rect.top - 22)
                    el.style.display = ''
                    el.style.webkitTransform = `translate3d(0,${y}px,0)`
                    el.style.transform = `translate3d(0,${y}px,0)`
                    let inner = el.getElementByClassName('inner-hook')[0]
                    inner.style.webkitTransform
                }
            }
        }
    },
    props: {
        'delivery-price': {
            type: Number,
            default: 0
        },
        'min-price': {
            type: Number,
            default: 0
        },
        'selected-goods': {
            type: Array,
            default() {
                return []
            }
        }
    },
    computed: {
        getTotalPrice() {
            let price = 0
            this.selectedGoods.forEach(item => {
                price += item.price * item.count
            })
            return price
        },
        getCount() {
            let count = 0
            this.selectedGoods.forEach(item => {
                count += item.count
            })
            return count
        },
        isGotoDelivery() {
            let price = 0
            this.selectedGoods.forEach(item => {
                price += item.price * item.count
            })
            if (price === 0) {
                return this.minPrice + `起送`
            } else if (price < 20) {
                return '还差￥' + (this.minPrice - price) + '起送'
            } else {
                return '去结算'
            }
        }
    }
}
</script>

<style lang="stylus">
.bannerWrapper
    display: flex
    position: fixed
    bottom: 0
    left: 0
    z-index: 99
    height: 3rem
    width: 100%
    background-color: #141d27
    font-size: 0
    .leftContent
        flex: 1
        .cartLogo
            display: inline-block
            height: 3.5rem
            width: 3.5rem
            margin: 0 12px
            border-radius: 50%
            background-color: #141d27;
            position: relative
            top: -10px
            .logo
                display: inline-block
                border-radius: 50%
                height: 2.75rem
                width: 2.75rem
                position: relative
                top: .375rem
                left: .375rem
                background-color: #2b343c
                text-align: center
                &.hasContent
                    background-color: rgb(0,160,220)
                    .icon
                        color:#fff
                .icon
                    display: inline-block
                    font-size: 1.5rem
                    color: rgba(255,255,255,0.4)
                    line-height: 2.75rem
        .num
            width: 1.5rem
            position: absolute
            left: 2.75rem
            top: -10px
            box-sizing: border-box
            border-radius: .375rem
            background-color: rgb(240,20,20)
            font-size: 9px
            font-weight: 700
            text-align: center
            color: #ffffff
            line-height: 1rem

        .totalPrice
            display: inline-block
            box-sizing: border-box
            padding-right: .75rem
            margin-top: .75rem
            border-right: 1px solid rgba(255,255,255,0.1)
            color: rgba(255,255,255,0.4)
            font-size: 1rem
            font-weight: 700
            line-height: 24px
            vertical-align: top
        .disc
            display: inline-block
            box-sizing: border-box
            padding-left: .75rem
            margin-top: .75rem
            color: rgba(255,255,255,0.4)
            font-size: 12px
            font-weight: 200
            line-height: 24px
            vertical-align: top
    .rightContent
        flex: 0 0 6.5625rem
        background-color: #2b333b
        text-align: center
        &.goal
            background-color: #00b43c
            .pricStart
                color: #ffffff
        .pricStart
            display: inline-block
            margin: 0 auto
            font-size: .75rem
            color: rgba(255,255,255,0.4)
            font-weight: 700
            line-height: 3rem
    .ball-content
        .ball
            position: fixed
            left: 2rem
            bottom: 1.375rem
            z-index 99
            .inner
                height: 1rem
                width: 1rem
                border-radius: 50%
                background-color: rgb(0,160,220)
                transition: all 0.4s
        .drop-enter-active
            transition: all 0.4s
        .drop-enter
            transform: translate3d(99px,0,0)
</style>

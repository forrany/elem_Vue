<template>
    <div class="header">
        <div class="content-wrapper">
            <img :src="seller.avatar" alt="avatar" id="avatar">
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="number">
                    <span class="count">{{seller.supports.length}}个</span>
                    <i class="icon-keyboard_arrow_right"></i>
                </div>
                <div class="deliver">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}} </span>
                </div>
            </div>
            <div class="background">
                <img :src="seller.avatar" width="100%" height="100%">
            </div>
        </div>
        <div class="notice-wrapper">
            <div class="notice">
                <span class="icon"></span><span class="text">{{seller.bulletin}}</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
    </div>
</template>
<script>
/* eslint-disable */
export default {
    props: {
        seller: {
            type: Object
        }
    },
    created() {
        this.classMap = ['decrease','discount','special','invoice','guarantee']
    }
}
</script>
<style lang="stylus">
    @import '../../common/stylus/mixin'
    .header
        background: rgba(7,17,27,0.5) 
        .content-wrapper
            padding: 1.5rem .75rem 1.125rem 1.5rem
            font-size: 0
            position relative
            #avatar
                width: 4rem
                height: 4rem
                border-radius: .125rem
                vertical-align: top
            .content
                display:inline-block
                background-color: "#FFFFFF"
                height: 100%
                margin-left: 1rem
                color: rgb(255,255,255)
                .title
                    margin: .125rem 0 .5rem 0
                    .brand
                        display: inline-block
                        vertical-align: top
                        width: 1.875rem
                        height: 1.125rem
                        bg-image('brand')
                        background-size: 1.875rem 1.125rem
                        background-repeat: no-repeat
                    .name
                        display: inline-block
                        margin-left: .375rem
                        font-size: 1rem
                        line-height: 1.125rem
                        font-weight: 600
                .number
                    position absolute 
                    height 1.5rem
                    right .75rem
                    bottom .875rem  
                    padding 0 .5rem
                    border-radius .875rem
                    background-color:rgba(0,0,0,0.2) 
                    text-align center        
                    .count
                        vertical-align top
                        font-size .75rem
                        line-height 1.5rem
                    .icon-keyboard_arrow_right
                        font-size .75rem
                        line-height 1.5rem
                        margin-left .125rem
                    
                .deliver
                    font-size: .75rem
                    line-height: .75rem
                    margin-bottom: .625rem
                .support
                    margin-bottom: .125rem
                    height .75rem
                    .icon
                        display inline-block
                        width: .75rem
                        height: .75rem
                        line-height: .75rem
                        background-size: .75rem .75rem
                        background-repeat: no-repeat
                        vertical-align: top
                        &.decrease
                            bg-image('decrease_1')
                        &.discount
                            bg-image('discount_1')
                        &.guarantee
                            bg-image('guarantee_1')
                        &.invoice
                            bg-image('invoice_1')
                        &.special
                            bg-image('special_1')
                    .text
                        display: inline-block
                        margin-left: .25rem
                        font-size: .625rem
                        line-height: .75rem
                        margin-top: .125rem
            .background
                position: absolute 
                top: 0
                left: 0
                z-index: -1
                width: 100%
                height: 8.375rem
                filter: blur(10px)
        .notice-wrapper
            height: 1.75rem
            position: relative
            background-color:rgba(7,17,27,0.2)
            .notice
                color: rgb(255,255,255)
                height: 1.75rem
                line-height 1.75rem
                padding 0 1.375rem 0 .75rem
                white-space nowrap  
                overflow hidden
                text-overflow ellipsis
                .icon
                    display: inline-block
                    width: 1.375rem
                    height: .75rem
                    line-height: 1.75rem
                    bg-image('bulletin')
                    background-size: 1.375rem .75rem
                    background-repeat: no-repeat
                    vertical-align: top
                    margin-top: .375rem
                .text
                    font-size: .625rem
                    margin: 0 .25rem
                    vertical-align: top
                    text-align: center
                .icon-keyboard_arrow_right
                    font-size: 0.625rem
                    line-height: 1.75rem
                    position: absolute 
                    right: .75rem
</style>

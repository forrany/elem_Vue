# control组件 
## 样式相关
![购物车详情页](https://github.com/forrany/elem_Vue/blob/master/sell/resource/%E5%A4%96%E5%8D%9601_%E5%95%86%E5%93%81%E9%A1%B5.jpg)

根据需求，可以看到`control`组件主要是为购物车添加、删除而制作的，且在多处复用。

从样式可以很简单的分成三个部分
```html
<div class="minus"> </div>
<div class="content"> </div>
<div class="plus"> </div>
```
横向排列，只需要使用`display:inline-block`即可

这里简单说下样式的几个注意点：
- inline-block，会有inline的特性，注意设置`text-align:center`来使其水平居中
- 使用`verticle-align:top`使其上对其
- `line-height`设置一致来配合`verticle-align`实现居中对齐

## vue相关/逻辑设计

`control`组件是嵌入在`food-item`之中的，而购物车`showcart`又需要传入`selected-good`来实现结账逻辑。
所以，这里在遍历`food-item`的时候，把每一个`food`信息和状态传入`contorl`。

`control`要能够改变food的状态，渲染自己的UI

### 传入food
遍历`foot-item`的时候，就会得到food，然后通过`<cartControl :food="food"/>`传入food

在组件中，增加商品的逻辑如下
```javascript
if(!this.food.count){
    this.food.count = 0;
}else {
    this.food.count++
}
```
也就是，第一次传入时，是没有count选项的，因此只需要设置，如果有了，就++。

**但是，如果这样写，不会触发渲染。**
> 如果需要增加和更改属性，并希望UI渲染，必须使用VUE中规定的'特殊方法'

```javascript   
import Vue from 'vue'
if(!this.food.count){
    Vue.set(this.food, 'count', 1)
}else {
    this.food.count++
}
```
这样，通过set增加属性，就可以被vue检测到 ，进行动态的UI更新了。

## 与购物车组件的联动
组件的动态联动和控制，主要通过`selected-goods`实现。 

`control`与`shopcart`的共同父组件是`Goods`，该组件会向购物车绑定`selected-goods`，因此这里写一个计算方法
```javascript   
computed: {
    selectedGoods() {
        let returnGoods = [];
        this.goods.forEach( good => {
            good.foods.forEach( food => {
                if(food.count > 0) {
                    returnGoods.push({
                        'price': food.price,
                        'count': food.count
                    })
                }
            });
        });
    }
}
```
至此，就可以实现联动了。
## 简单的动画效果
关于vue的动画效果，主要参考vue官方文档。这里简单做一个总结：
- v-enter 是最开始的状态，可以理解为动画开始的状态
- v-enter-active 经过最开始状态后，就开始动画激活的状态
- v-leave-active 这是关闭、结束动画之前的状态
- v-leave-to 这是最后动画完毕的状态

因此，一般如果是一个具有往复效果的过渡，会把`v-enter-active`与`v-leave-active`写在一起，都是动画的过程，或者是动画结束后的状态。

而`v-enter`与`v-leave-to`是动画还没开始的，初始状态，可以写在一起
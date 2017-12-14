# swiper

小程序自定义轮播;<br>
本来要做成旋转木马那样循环轮播，但是没思路了，本人才疏学浅，如果有大神看到可以指点一下。<br>
欢迎 fork 指正！！！<br>


样式展示

![](https://github.com/sun-zw/swiper/blob/master/images/IMG_20171214_100734.png)
<br>
![](https://github.com/sun-zw/swiper/blob/master/images/IMG_20171214_100809.png)
<br>

代码：
```html
<view class="show_swiper" style='width: {{winWidth}}px;height: {{winHeight}}px;'>
  <view class='show_swiper_list'
        bindtouchstart='swiperTouchstart'
        bindtouchmove='swiperTouchmove'
        bindtouchend='swiperTouchend'
        style='width: {{allWidth}}px;position: relative;left: {{(winWidth-itemWidth)/2}}px;'>

    <block wx:for="{{swiperList}}">
       <view class='swiper_item' 
            data-curid="{{curIndex}}" 
            data-index='{{index}}' 
            animation="{{curIndex == index? animationToLarge : animationToSmall}}" 
            style='width: {{itemWidth}}px;height: {{itemWidth*1.4}}px;transform: scale({{curIndex == index ? 1 : scale}});-'>

          {{item}}
          
      </view>
    </block>

  </view>
</view>

```

主要事件；
```javascript

  //触摸开始的事件
  swiperTouchstart: function (e) {
    // console.log('touchstart',e);
    let startClinetX = e.changedTouches[0].clientX;
    this.setData({
      startClinetX: startClinetX, //触摸开始位置；
      startTimestamp: e.timeStamp, //触摸开始时间；
    })
  },

  //触摸移动中的事件
  swiperTouchmove: function (e) {
    // console.log('touchmove',e);
  },

  //触摸结束事件
  swiperTouchend: function (e) {
    // console.log("触摸结束",e);

    let times = e.timeStamp - this.data.startTimestamp, //时间间隔；
        distance = e.changedTouches[0].clientX - this.data.startClinetX; //距离间隔；
    //判断
    if (times < 500 && Math.abs(distance) > 10) {
      let curIndex = this.data.curIndex;

      let x0 = this.data.itemWidth,x1 = this.data.translateDistance,x = 0;
      if ( distance > 0) {
       
        curIndex = curIndex - 1
        if(curIndex < 0){
          curIndex = 0;
          x0 = 0;
        }
        x = x1 + x0;
      } else {
      
        // console.log('+1',x);
        curIndex = curIndex + 1
        if (curIndex >= this.data.swiperList.length) {
          curIndex = this.data.swiperList.length-1;
          x0 = 0;
        }
        x = x1 - x0;
      }
      this.animationToLarge(curIndex, x);
      this.animationToSmall(curIndex, x);
      this.setData({
        curIndex: curIndex,
        translateDistance: x
      })
      
    } else {
      
    }
  },
  // 动画
  animationToLarge: function (curIndex,x) {
   
    this.animation.translateX(x).scale(1).step()
    this.setData({
      animationToLarge: this.animation.export()
    })
  },
  animationToSmall: function (curIndex,x) {

    this.animation.translateX(x).scale(0.7).step()
    this.setData({
      animationToSmall: this.animation.export()
    })
  },

```


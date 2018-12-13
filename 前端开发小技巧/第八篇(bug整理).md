## safari 弹框内容比较复杂输入框出现双光标
上一个光标卡住，在切换输入框时才消失

解决方法 添加属性
```
.{
   transfrom:translate3d(0,0,0);
}
``` 
这样设置浏览器会开启GPU硬件加速模式

特别是在有页面动画时会有很明显的顺畅提升，特别是在移动端

开启所有浏览器的GPU加速
```
webkit-transform: translateZ(0);
-moz-transform: translateZ(0);
-ms-transform: translateZ(0);
-o-transform: translateZ(0);
transform: translateZ(0);
```
或
```
webkit-transform: translate3d(0,0,0);
-moz-transform: translate3d(0,0,0);
-ms-transform: translate3d(0,0,0);
-o-transform: translate3d(0,0,0);
transform: translate3d(0,0,0);
```
看一篇博客说道加上这个属性要注意的几点

ps：以下为转发

[源地址](http://blog.bingo929.com/transform-translate3d-translatez-transition-gpu-hardware-acceleration.html)



![企业微信截图_b7ccb02b-0b70-4ad7-a4df-0d03595eb567.png](https://upload-images.jianshu.io/upload_images/3623627-bd133103005a984d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![企业微信截图_4d258b93-8d4a-4505-9c87-8753d20be7a0.png](https://upload-images.jianshu.io/upload_images/3623627-3220cb731c39e7c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 后端重定向回来 vue-router不刷新
bug流程：

```打开网页->无授权信息->跳转后端页面授权->重定向回login界面->界面空白```

经过加断点测试 原因在第一次进入login获取信息 被后端拦截重定向回login时 vue-router认为页面没有跳转导致不会重新渲染login界面


解决方法
```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

## 页面关闭时弹出是否确认关闭此页面

在window 对象上添加事件onbeforeunload、
nload 在body被卸载时触发
```
    <script type="text/javascript">
      window.onbeforeunload = window.onunload  = function(){
        return "确认退出吗"
      }
    </script>
```
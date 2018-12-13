# 第二篇
## 1.使用this无法访问Vue实例
setTimeout/setInterval this指向改变，无法用this访问Vue实例
```
mounted(){
    setTimeout(function () { //setInterval  
        console.log(this);//  this  Window  
    },1000);
}
```
解决方法：使用箭头函数或者缓存this
```
//箭头函数访问this实例，因为箭头函数本身没有绑定this
setTimeout(() => {
    console.log(this);
}, 500);
//      this  
let self=this;
setTimeout(function () {
    console.log(self);//  self    this  
},1000);
```
## 2.定时器销毁
setInterval路由跳转继续运行没有销毁

> 场景：比如一些弹幕，走马灯文字，这类需要定时调用的，路由跳转之后，因为组件以及销毁了，但是setInterval还没有销毁，还在继续后台调用，控制台会不断报错，如果运算量大的话，无法及时清除，会导致页面卡顿。

解决方法：在组件生命周期beforeDestroy停止setInterval
```
 beforeDestroy(){
 //我通常是吧setInterval()定时器赋值给this实例，然后就可以像下面这么暂停。                
 clearInterval(this.intervalid);
 
```

## 3.watch部分使用方法
可使用这种方法给watch添加监听方法，不过需要注意在使用keepalive时容易多次绑定，导致一次数据变化执行多次监听函数, 解决方法：
```
data(){
    return{
        isInit: false,
    }
}

method:{
    init(){
        var self = this;
        if (!self.isInit) {
            self.$watch("sheetIndex", function(val, oldval) {
                
            })
        }
    }
}
```
判定```isInit```是否存在来判定页面是否被销毁过

#### watch 深度watch与立即触发回调

- 选项：deep
  在选择参数中指定deep:true，可以监听对象中子属性的变化。
- 选项：immediate
  在选项中指定immediate:true，将立即以表达式的当前值触发回调，也就是默认触发一次

```
watch: {
    obj: {
      handler(val, oldVal) {
          console.log('属性发生变化触发这个回调',val, oldVal); },
      deep: true //监听这个对象中的每一个属性变化
    },
    step: { //    属性
      //watch
      handler(val, oldVal) {
        console.log("默认触发一次", val, oldVal); 
      },
      immediate: true //默认触发一次
    },
},
```
 

## 4.Vue数组/对象更新，视图不更新
```
场景：很多时候我们习惯于这样操作数组和对象
export default {
  data() { // data   return {
    arr: [1, 2, 3],
    obj: {
      a: 1,
      b: 2
    }
  };
},
//数据更新，数组视图不更新
this.arr[0] = 'OBKoro1'; 
this.arr.length = 1; 
console.log(arr);// ['OBKoro1']; 
//数据更新，对象视图不更新              
this.obj.c = 'OBKoro1';
delete this.obj.a;
console.log(obj); // {b:2,c:'OBKoro1'}
```
==解决方案==
- this.$set(你要改变的数组/对象，你要改变的位置/key,你要改变成是， value)
- 数组原生方法触发视图更新
- 整体替换数组/对象


## HTML条件注释判断

符号 | 范例 | 	说明
---|---|---
|!	| [if !IE] | NOT运算符。这是摆立即在前面的功能，操作员，或子表达式扭转布尔表达式的意义。|
lt | [if lt IE 5.5] | 小于运算符。如果第一个参数小于第二个参数，则返回true。
lte | [if lte IE 6] | 小于或等于运算。如果第一个参数是小于或等于第二个参数，则返回true。
gt | [if gt IE 5] | 大于运算符。如果第一个参数大于第二个参数，则返回true。
gte | [if gte IE 7] | 大于或等于运算。如果第一个参数是大于或等于第二个参数，则返回true。
( ) | [if !(IE 7)] | 子表达式运营商。在与布尔运算符用于创建更复杂的表达式。
& | [if (gt IE 5)&(lt IE 7)] | AND运算符。如果所有的子表达式计算结果为true，返回true
	&#166; |[if (IE 6)&brvbar;(IE 7)] | OR运算符。返回true，如果子表达式计算结果为true。
	
	只有低于ie10 才会执行中间代码

```
  <!--[if lt IE 10]>
    <script type="text/javascript">
      document.write("<div style='position:fixed;top:0;left:0;right:0;bottom: 0;z-index:2000;width:100%;height:100%;padding-top:200px;background-color:rgb(235,235,235)'><img style='position:absolute;top:20%;left:50%;margin-left:-50px;height:100px;width:100px' src='static/ie.png' /><P style='position: absolute;top:40%;left:50%;margin-left:-250px;font-size:20px;width:500px;'>我知道您念旧，但为了更好的产品体验，建议您升级浏览器或者使用谷歌浏览器打开</P></div>")
    </script>
  <![endif]-->
```
ps：ie10和11已经不支持<if IE>


## index.html 中调用img 的src路径
在vue-cli 生成的项目中 如想在index.html中调用img
如使用常规方法调用 ```src/assets/img``` 调用不到图片 可把图片放在static文件中调用

ps: 做ie9的升级浏览器提示，vue在ie9及以下不执行，原先想放在App.vue 中 使用v-if 做显隐判断，结果当然不可行...... 所以放在index.html中 但是不管怎么试路径 图片都无法显示，查了一下  assets目录下的文件会被webpack处理，打包之后不是这个路径了。需要把你的图片放到static目录下。对于的src的路径写成/static/xxxxxx

## 居中样式
原本在ie9中使用
```
.{
    position: absolute;
    width:100px;
    height:100px;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%)
}
```
但不兼容，查了下文档
```
Internet Explorer 10、Firefox、Opera 支持 transform 属性。

Internet Explorer 9 支持替代的 -ms-transform 属性（仅适用于 2D 转换）。

Safari 和 Chrome 支持替代的 -webkit-transform 属性（3D 和 2D 转换）。

Opera 只支持 2D 转换。
```

也木有效果，又试了下使用margin:auto; 居中
```
<div style="text-align:center"> 
<div style="margin-left:auto ; margin-right:auto"></div> 
</div> 
```
还是木有用
最后简单粗暴
```
.{
    position: absolute;
    width:100px;
    height:100px;
    top:50%;
    left:50%;
    margin-top:-50px;
    margin-left:-50px;
}
```
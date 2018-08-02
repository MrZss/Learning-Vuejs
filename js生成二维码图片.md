(vue)
使用qrcodejs2
```
npm install qrcodejs2 --save
```
页面中引入
```
import QRCode from 'qrcodejs2'

components: {QRCode}
```
页面填充
```
<div id="qrcode" ref="qrcode"></div>
```
页面调用
```
qrcode () {
  let qrcode = new QRCode('qrcode', {  
      width: 232,  // 设置宽度 
      height: 232, // 设置高度
      text: 'https://baidu.com'
  })  
},
/*
    @  在需要调用的地方  这样必须这样调用  否则会出现  appendChild  null  就是id为qrcode的dom获取不到 返回结果为null
*/
this.$nextTick (function () {
   this.qrcode();
})
```
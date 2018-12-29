#### 最开始的需求是 多张图拼成一张图 并且长按保存

> 最开始的愚见：
>
> ​     因为要等全部的图片渲染完成在区生产canvas 我就只在最大的呢张图片上加了onload事件
>
> ​     这样图片加载慢 也能最后执行生成canvas
>
> 今天看了一个小想法，发现自己真蠢：
>
> ​     一张张递归渲染到指定位置不就好了
>
> ​    唉 果真还是垃圾宝宝一个

```javascript
draw(){
    let data = ['img_url_1','img_url_2','img_url_3','img_url_4','img_url_5']
    //数组存放url
    let canvas = document.createElement('canvas')
    // 创建canvas 画板
    let context = canvas.getContext('2d')
    canvas.width=290;
	canvas.height=290;
	context.rect(0,0,canvas.width,canvas.height)
	//绘制空白矩形区域
	function drawing(n){
        if(m<len){
            let img = new Image;
            //img.crossOrigin = 'Anonymous'; //解决跨域
            img.src=data[n];
            img.onload=function(){
                //使用递归 把每一张图一一按照位置渲染上去
			    ctx.drawImage(img,0,0,290,290);
			    drawing(n+1);//递归       
		    }
        }else{
			//保存生成作品图片
			base64.push(c.toDataURL("image/jpeg",0.8));
			//alert(JSON.stringify(base64));
			fn();
		}
        drawing(0);
	}
}
```
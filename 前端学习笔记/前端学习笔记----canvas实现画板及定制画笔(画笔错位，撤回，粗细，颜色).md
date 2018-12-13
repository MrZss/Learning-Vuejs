> 本期的小需求:   
> 在页面中实现一个能进行绘画的画板  
> 能实现画笔颜色、粗细、撤回等功能

### 开发环境
> Vue + elementUI

### 采坑记录

- 画笔与鼠标错位  （恶心很久）
- 画笔切换颜色 粗细，原来绘画的线条也改变了
- 图片跨域等等.....

### 动手
#### 1.生成或选中canvas对象
```
// html
<canvas id="box"></canvas>

// vue

let canvas = $("#canvas")[0]
//$("#canvas")获取的是jquery对象 不是真实的dom对象 要么使用dom操作获取

```

#### 2.生成绘画画布环境
```
let canvas = $("#canvas")[0]
//$("#canvas")获取的是jquery对象 不是真实的dom对象 要么使用dom操作获取
let ctx = canvas.getContext("2d")
//getContext() 方法返回一个用于在画布上绘图的环境。
//目前只接受 2d 这个参数
```
#### ps: canvas 画板绘制api
```
//矩形
ctx.fillRect(x, y, width, height)
//绘制一个填充的矩形

ctx.strokeRect(x, y, width, height)
//绘制一个矩形的边框

ctx.clearRect(x, y, widh, height)
//清除指定的矩形区域，然后这块区域会变的完全透明

//线条路径

ctx.beginPath()
//新建一条路径，路径一旦创建成功，图形绘制命令被指向到路径上生成路径

ctx.moveTo(x, y)
//把画笔移动到指定的坐标(x, y)。相当于设置路径的起始点坐标。

ctx.closePath()
//闭合路径之后，图形绘制命令又重新指向到上下文中

ctx.stroke()
//通过线条来绘制图形轮廓

ctx.fill()
//通过填充路径的内容区域生成实心的图形

// 绘制圆弧
ctx.arc(x, y, r, startAngle, endAngle, anticlockwise):
//以(x, y)为圆心，以r为半径，从tartAngle弧度开始到endAngle弧度结束。anticlosewise是布尔值，true表示逆时针，false表示顺时针。(默认是顺时针)
//这里的度数都是弧度。
//0弧度是指的x轴正方形
//radians=(Math.PI/180)*degrees   //角度转换成弧度
ctx.arcTo(x1, y1, x2, y2, radius):
//根据给定的控制点和半径画一段圆弧，最后再以直线连接两个控制点
```

#### 3.canvas 画板实现
> 实现思路： 监听鼠标事件，用drawCircle()方法画出来
> 1. 监听画板区域鼠标事件,未开始时设置painting 为 false
> 2. 当鼠标按下时(mousedown),开启画笔函数，painting为true,鼠标松开之前记录路径点,并绘制记录点
> 3. 使用lineTo()函数把每个点连接
> 4. 鼠标松开的时候（mouseup）,关闭画笔函数

```
data(){
    return{
        isClear：false
    }
}
canvasLoad(){
    let canvas = $("box")[0];
    let context = canvas.getContext("2d");
    //
    let img = new Image();
    img.src = "";
    //因为我这个画板是有背景图的所以需要加载img 如若不需要忽略此步
    img.onload = () =>{
        //使用onload函数 在img src加载完毕后调用
        let width = img.width
        let height = img.height
        //onload 后才能获取到图片尺寸
        canvas.width = width
        canvas.height = height
        // ****切忌使用css去指定canvas宽高****
        //css设置canvas元素宽高会导致像素点被压缩
        //导致canvas 元素点定位偏差 以及 画笔等错位
        //如需缩放 设置先设置img宽高 压缩 在把宽高赋予给canvas
        context.drawImage(img, 0, 0, width,height);
        //绘制背景图
        this.drawLine()
        // 定义画笔
        $("#blackBtn").click(() => {
            context.strokeStyle = "#000000";
        });
        $("#whiteBtn").click(() => {
            context.strokeStyle = "#FFFFFF";
        });
        $("#clearBtn").click(() => {
        // 清屏
            context.clearRect(0,0,innerWidth,innerHeight)
        });
        $("#eraserBtn").click(() => {
        // 橡皮擦
            this.isClear=true;
        });
        $("#widthBtn").click(() => {
        // 设置画笔宽度
        // 不是点击事件 可以改成监听等等
            context.lineWidth = width;
        });
        
        // 调整画笔颜色的点击事件可供参考
        // context需保持上下文一致
    }
}
drawLine() {
      const that = this
      let canvas = $("#box")[0];
      let context = canvas.getContext("2d");
      context.canvas.addEventListener("mousedown", startAction)
      // 监听画板鼠标按下事件 开始绘画
      context.canvas.addEventListener("mouseup", endAction)
      // 监听画板鼠标抬起事件 结束绘画
      function startAction(event) {
        //监听鼠标移动事件
        //如果没有使用橡皮擦就画线
        if (!that.isClear) {
          //开始新的路径
          context.beginPath();
          context.lineCap = "round";
          // 结束 及 开始 圆形线帽
          context.moveTo(event.pageX,event.pageY);
          context.stroke();
        }
        context.canvas.addEventListener("mousemove", moveAction);
      }
              //封装鼠标抬起函数
      function endAction() {
            //不再使用橡皮擦
            that.isClear=false;
            //移除鼠标移动事件
            context.canvas.removeEventListener("mousemove",moveAction);
      }
      function moveAction(event) {
            //判断是否启动橡皮擦功能
            if(that.isClear){
               context.clearRect(event.pageX-8,event.pageY-8,16,16);
            return;
            }
            context.lineTo(event.pageX,event.pageY);
            context.stroke();
       }
}

```
#### 4.实现撤回功能
```javascript
data(){
    return{
        ...
        canvasHistory
    }
}
.... 
canvasLoad(){
    ...
    $("#recallBtn").click(() => {
        // 撤回画板
        img.src = that.canvasHistory[that.canvasHistory.length-1]
        // 撤回回上一个版本
        //重新绘制过程
        this.canvasLoad()
        // 撤回完成后删除最后一项
        that.canvasHistory.pop()
    });
        
    ...
    
}
drawLine(){
     function startAction() {
     ...
        that.canvasHistory.push(canvas.toDataURL())
        //每次开始绘画前 把上一个版本的canvas转化为url放进数组中
     }
}
```


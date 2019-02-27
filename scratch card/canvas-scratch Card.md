# canvas-scratch Card

#### core: ３つの関数

1. **<font color='red'>touchstart</font>**
    event.changedTouches[0]    拿第一根指
   ctx.globalCompositeOperation = 'destination-out';  合成
   lineTo比moveTo多1即可
2.  **<font color='red'>touchmove</font>**
   基本同上 不过不用moveTo 直接lineTo即可
3. **<font color='red'>touchend</font>**
   通过flag标识来判断当前canvas里opacity == 0的像素的个数
   allPx = 图片长*宽
   通过判断getImageData().data[ 4 * i + 3 ]是否为0 来控制flag
   flag到达一定值时 allPx opcaity变为0

```javascript
window.onload = function () {
    var canvas = document.querySelector('#test')
    canvas.width= document.documentElement.clientWidth;
    canvas.height= document.documentElement.clientHeight;

    if (canvas.getContext) {
        var ctx = canvas.getContext('2d');
        // 获取画笔
        var img=new Image();
        img.src="bird-img/a.png"

        img.onload=function () {
            var flag = 0 ;
            ctx.drawImage(img,0,0,canvas.width,canvas.height)
            canvas.addEventListener("touchstart",function () {

                var touch = event.changedTouches[0];

                var x=touch.clientX - canvas.offsetLeft;
                var y=touch.clientY - canvas.offsetTop;

                ctx.save();
                ctx.lineCap = "round";
                ctx.lineJoin = "round"
                ctx.lineWidth=60;
                ctx.globalCompositeOperation = 'destination-out';
                ctx.beginPath();
                // ctx.arc(x,y,10,0,2*Math.PI);
                ctx.moveTo(x,y);
                ctx.lineTo(x+1,y+1);
                ctx.fill();
                ctx.restore();
            })
            canvas.addEventListener("touchmove",function () {

                var touch = event.changedTouches[0];

                var x=touch.clientX - canvas.offsetLeft;
                var y=touch.clientY - canvas.offsetTop;

                ctx.save();
                ctx.lineCap = "round";
                ctx.lineJoin = "round"
                ctx.lineWidth = 60
                ctx.globalCompositeOperation = 'destination-out'

                // ctx.arc(x,y,10,0,2*Math.PI);

                ctx.lineTo(x,y);
                ctx.stroke();
                ctx.restore();
            })
            canvas.addEventListener("touchend",function () {

                var imgData = ctx.getImageData(0,0,canvas.clientWidth,canvas.clientHeight);
                var allPx = imgData.width * imgData.height;
                console.log(allPx);
                for(var i=0;i<allPx;i++){
                    if(imgData.data[4*i+3]===0){
                        flag++;
                    }
                }
                console.log(flag)
                if(flag>=allPx){
                    canvas.style.opacity = 0;
                }
            })
            // console.log(canvas.toDataURL());
        }
    }
}
```
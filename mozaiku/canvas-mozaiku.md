# canvas-mozaiku

1. core：
    **<font color='red'>ctx. getImageData(x,y,w,h)</font>**

   **<font color='red'>ctx.createImageData(w,h)</font>**         参数也可以是第一个的返回值

   **<font color='red'>ctx.putImageData(第一个的返回值,x,y)</font>**

2.  **ctx. getImageData()**.width/height
                                         .data    <-----return一个伪arr 
   其中[ 4 * i  + 0] 是R
          [ 4 * i  + 1 ] 是G
            [ 4 * i + 2 ] 是B
          [ 4 * i  + 3] 是A                所以此arr共有**4倍(width*height**)个数据

3. Define两个函数
   一个 get old pic all colors
   一个set colors for new(mozaiku) pic    

4. 根据mozaiku size来用双层for切割old pic

5. 在一个mozaiku block中random抽取一组座標（x,y）并且get  their color[ r,g,b,a]

6.  将这组坐标所在的blcok中的all pixel 统一为这组坐标的color

7. 在最外面 putImageData （画图）

```javascript
//需要的関数
function getColor(imgData,x,y){
    var color = [];
    color[0]=imgData.data[(y*imgData.width+x)*4+0];
    color[1]=imgData.data[(y*imgData.width+x)*4+1];
    color[2]=imgData.data[(y*imgData.width+x)*4+2];
    color[3]=imgData.data[(y*imgData.width+x)*4+3];

    return color;
}
function setPx(imgData,x,y,color) {
    imgData.data[(y*imgData.width+x)*4] = color[0];
    imgData.data[(y*imgData.width+x)*4+1] = color[1];
    imgData.data[(y*imgData.width+x)*4+2] = color[2];
    imgData.data[(y*imgData.width+x)*4+3] = color[3];

}
//
    var canvas = document.querySelector('canvas');
    if(canvas.getContext){
        var ctx = canvas.getContext('2d');
        var img = new Image();

        img.src = "bird-img/queen.png";  //set pic src
        img.onload = function () {
            ctx.drawImage(img,0,0,682,600);
            var old = ctx.getImageData(0,0,682,600);
            var neww = ctx.createImageData(682,600);
            var size =10;
            for(var i=0;i<682/size;i++){
                for(var j=0;j<600/size;j++){
                var x,y;
                x = Math.floor(Math.random()*size);
                y = Math.floor(Math.random()*size);
                var random_c = getColor(old ,x+i*size,y+j*size);

                   for(var a=0;a<size;a++){
                       for(var b=0;b<size;b++){
                           setPx(neww,a+i*size,b+j*size,random_c)
                       }
                   }
                }
            }
            ctx.putImageData(neww,618,0);
        //     ctx.putImageData(neww,0,0);

        }
    }
```
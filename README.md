# dicom文件在js中通过嵌入着色器语言GLSL实现WebGL渲染
# 为什么不通过Canvas进行dicom渲染，是因为医学图像数据通常比较大、复杂，需要实时渲染并进行交互操作。WebGL具有高效、可扩展、交互性强等特点，能够满足医学图像数据的需求。通过WebGL技术，可以实现高精度的体绘制、调节、切割、3D重建等功能。
# 先进行纹理的初始化
# 配置纹理供WebGL使用
``` 
  var canvas = document.createElement("canvas");
  var gl = canvas.getContext("experimentation-webgl") || canvas.getContext("webgl");
  var textureObj  = gl.createTexture();//创建纹理对象
  //开启纹理单元；至少支持8个纹理单元编号；通过纹理单元来操作纹理对象
 gl.activeTexture(gl.TEXTURE0);
 gl.bindTexture(gl.TEXTURE_2D, textureObj);
 //对纹理图像进行Y轴反转；WebGL纹理坐标系统中的t轴方向和图片坐标系统的Y轴方向是相反的，反转之后才能映射到图像上
 gl.pixelStorei(gl.UNPACK_FLIP_Y_WEBGL,true);
 gl.piexlStorei(gl.UNPACK_ALINGMENT,true);
 // 将纹理图像分配给对象
 gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGB, texWidth, texHeight, 0, gl.RGB, gl.UNSIGNED_BYTE, texData);
  // texParameteri函数：如何根据纹理坐标获取纹理颜色，按哪种方式重复填充纹理的参数设置
  // TEXTURE_MAG_FILTER 放大方法：纹理的绘制范围变大时，如何获取纹理颜色；指明填充扩大造成像素间隙的方法
  // TEXTURE_MIN_FILTER 缩小方法：纹理的绘制范围变小时，如何获取纹理颜色；指明缩小剔除部分像素的方法
  // NEAREST: 使用原纹理上距离映射后像素（新像素）中心最近的那个像素的颜色值作为新像素的值
  // LINEAR: 使用距离新像素中心最近的四个像素的颜色值的加权平均，作为新像素的值：该方法更好，开销较大
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.NEAREST);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
   //TEXTURE_WRAP_S 水平填充：对纹理图像左侧或右侧的区域进行填充
  //TEXTURE_WRAP_T 垂直填充：对纹理图像上方和下方的区域进行填充
  //CLAMP_TO_EDGE: 使用纹理图像边缘值
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_S, gl.CLAMP_TO_EDGE);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_T, gl.CLAMP_TO_EDGE);
```

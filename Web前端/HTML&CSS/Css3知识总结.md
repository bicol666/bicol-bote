# Css3知识总结

# 一、平面转换（transform）

- 平面转化是一个**复合属性**，后面可以跟多个属性，中间以空格隔开；

例如：transform: translate(10px,20px)  scale(1.2)；-->表示既要在x轴和y轴方向上分别位移10、20像素的距离，还要放大为1.2倍；



- **转换原点**（transform-origin）

  转换原点是元素发生平面转换（位移、缩放、旋转……）的**参考点**；

  

- **注意**

  一个元素若是添加了多个transform属性会发生**层叠效果**，可能会导致效果与我们所想的有差别；



### 1、位移（translate）

- **语法**

  - transform：translate( x轴位移距离 , y轴位移距离 )；

  - 也可只传入一个值，表示x轴的位移距离：translate( X轴位移距离 )；

  - 也可单独在X和Y轴方向上位移：translateX( X轴位移距离 )、translateY( Y轴位移距离 )；

    

- **可传入的值**

  - 像素坐标
  - 百分比：这个百分比参考的是元素自身的宽高尺寸；



- **详细分析**

  原有这样布局：

  ![](D:\Nanum-Note\Web前端\HTML&CSS\img\transform-translate-001.png)

  假设对这个蓝色的盒子(div)加上属性：transform: translate( 100px , 100px )；

  <img src="D:\Nanum-Note\Web前端\HTML&CSS\img\transform-translate-002.png" style="zoom: 80%;" />

  

  详情分析：

  <img src="D:\Nanum-Note\Web前端\HTML&CSS\img\transform-translate-detail.png" />

  

  位移得弄清楚元素所在的坐标轴才能更好地理解元素是怎么位移的。可以看见的是css中的坐标轴如上图所示，与一般我们接触到的坐标轴有差别，**Y轴向下方向为正**，所以translate（ x ，y）传入的值（坐标）准确的说是从元素原本所在的坐标轴的原点（0,0）到要移动的终点坐标；
  
  

### 2、缩放（scale）

缩放就是讲一个元素放大缩小，默认的转换原点是元素的中心点，也就是从元素的正中心向四周进行放大或缩小操作；



- **语法**

  transform: scale(放大/缩小倍数)；






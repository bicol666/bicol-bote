# CSS3总结

---

# 一、transition

> 元素样式发生改变效果是非常突兀的，`transition`用于指定元素样式变化时过渡效果。

### 说明


| 属性                       | 作用                                        |
| -------------------------- | ------------------------------------------- |
| transition                 | 设置元素过渡效果                            |
| transition-property        | 设置要过渡的属性                            |
| transition-duration        | 设置过渡持续时间                            |
| transition-timing-function | 设置时间曲线上的过渡速度。                  |
| transition-delay           | 设置过渡效果延迟时间，默认为0表示立即开始。 |



### transition

默认值：`all 0 ease 0`

继承性：无

语法：

```css
transition: property duration [timing-function] [delay] ;
```

> **注意：**
>
> 1、`transition-property`和`transition-durantion`是必须填写项。始终指定`transition-duration`属性，否则持续时间为`0`，`transition`不会有任何效果。
>
> 2、若要设置多个属性的过渡效果请使用`逗号,`分隔。	示例：`transition : width .5s , height 1s ;`
>
> 3、`property`设置为`all`表示为所有属性设置过渡效果，万金油。



### transition-property

语法

```css
transition-property: none|all| property;
```

取值说明

| 值       | 效果                                                         |
| -------- | ------------------------------------------------------------ |
| none     | **所有属性**都无过渡效果                                     |
| all      | **所有属性**都有过渡效果，但需要样式发生变化才会生效。       |
| property | **指定的属性**会有过渡效果。可以是单个CSS属性，也就可以是CSS属性列表，以`逗号,`分隔。 |





### transition-duration



### transition-timing-function



### transition-delay
# SPI总结.md

# 简介

> SPI（Service Provider Interface），简单来说就是可以根据接口获取对应的实现类实例，而这个类可以有多种选择。
>
> SPI可以帮助框架进行很好的扩展。



# Java SPI

> 根据接口找实现类。

将接口的实现类描述出来：约定的方式。

在resources下新建META-INF/services
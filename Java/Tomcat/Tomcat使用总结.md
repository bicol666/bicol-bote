# Toamcat使用总结

## 一、问题总结

### 1. Tomcat控制台输出乱码问题

![capture_20210317183440873](img\capture_20210317183440873.bmp)

这是由于Toamcat默认使用UTF-8编码，而控制台窗口使用了GBK编码，二者冲突导致的。



解决方法：

- 在Tomcat主目录下的conf目录下找到"logging.properties"文件并用文档编辑工具打开该文档。

![capture_20210317182445259](img\capture_20210317182445259.bmp)



- 找到"java.util.logging.ConsoleHandler.encoding="字段，将等号后面的值改为Idea对应的编码即可。

![capture_20210317182708270](img\capture_20210317182708270.bmp)
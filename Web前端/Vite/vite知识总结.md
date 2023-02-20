# vite知识总结.md

# 一、依赖预构建

当我们导入第三方依赖时，JS源码是这样的：

![image-20230108101011250](D:\bicoll-note\WEB前端\Vite\vite知识总结-img\image-20230108101011250.png)

经过`vite`进行依赖预构建之后：

![image-20230108111715357](D:\bicoll-note\WEB前端\Vite\vite知识总结-img\image-20230108111715357.png)

对于导入的第三方依赖，`vite`会对第三方依赖进行`依赖预构建`，构建完成之后生成的结果会被放到目录`/node_modules/.vite/deps`之下。我们源码中导入的`lodash`实际上是`vite`进行依赖预构建之后的结果。



#  二、配置

### 1、环境变量

> 相关配置项：
> - envPrefix：加载以 `envPrefix` 开头的环境变量，可以是一个字符串或数组。
>   - `envPrefix` 不应被设置为空字符串 `''`，这将暴露你所有的环境变量，导致敏感信息的意外泄漏。 检测到配置为 `''` 时 Vite 将会抛出错误。
>
> - envDir：用于加载 `.env` 文件的目录。可以是一个绝对路径，也可以是相对于项目根的路径。
>

配置了这两个选项之后`vite`就会在指定目录下加载`.env.product`、`.env.development`、`.env.local`、`.env`这四个文件里的环境变量，然后通过 import.meta.env 暴露在你的客户端源码中。



**关于import.meta.env**

源码：

![image-20230108111120535](D:\bicoll-note\WEB前端\Vite\vite知识总结-img\image-20230108111120535.png)

`vite`处理之后：

![image-20230108111225967](D:\bicoll-note\WEB前端\Vite\vite知识总结-img\image-20230108111225967.png)

可以发现在用到`import.meta.data`的地方`vite`直接会将环境变量注入到`import.meta.data`。



### 2、不同环境下的配置

首先为不同的环境创建不同的配置`vite`配置文件。

- vite.dev.config.ts	`开发环境`
- vite.prod.config.ts  `生产环境`
- vite.base.config.ts  `基础配置`
- vite.config.ts  `最终的配置`

`vite.config.ts`0

```ts
import { fileURLToPath, URL } from "node:url";
import { AntDesignVueResolver } from "unplugin-vue-components/resolvers";
import Components from "unplugin-vue-components/vite"; // Vue 的按需组件自动导入。
import { defineConfig, loadEnv } from "vite";
import vue from "@vitejs/plugin-vue";
import devConfig from "./vite.dev.config"; // 开发环境配置
import prodConfig from "./vite.prod.config"; // 生产环境配置
import baseConfig from "./vite.base.config"; // 基础配置

// 策略模式
const envResolver = {
  build: () => {
    console.log("生产环境");
    return { ...baseConfig, ...prodConfig };
  },
  serve: () => {
    console.log("开发环境");
    return { ...baseConfig, ...devConfig };
  },
}

export default defineConfig(({ command, mode }) => {
  console.log("command-->", command, "\tmode-->", mode);
  // 加载环境变量
  //检查process.cwd()路径下.env.development.local、.env.development、.env.local、.env这四个环境文件
  loadEnv(mode, process.cwd());

  return envResolver[command]();
});

```

### 3、静态资源处理

- 1、图片

  这种方式导入进来的是图片所在的`绝对路径`。

  ```js
  import loginBackground from '@/assets/images/login_background.svg'
  console.log(loginBackground); // --》/src/assets/images/login_background.sv
  ```

- 2、JSON文件

  JSON文件被导入后会**自动解析**成一个`对象`，而不是字符串。

  ```js
  
  ```

  支持按需导入。`vite`对于按需导入的内容，在`build`时会做`摇树优化`。

  ```js
  import { name } from './userInfo.json'
  ```

### 4、别名





# 三、`vite`开发服务器原理

在开发期间 Vite 是一个服务器，而 `index.html` 是该 Vite 项目的入口文件。

### 1、对`.vue`文件的处理

找到

### 2、对`.css`文件的处理

#### 处理过程

> `vite`在JS里发现导入了`.css`文件之后会去加载这个文件到内存，然后**转换为JS脚本**。浏览器发现要导入css文件就会向vite开发服务器去请求这个css文件，服务器虽然返回的文件名后缀是`.css`，但是会设置响应头`Content-Type:text/javascript`，让浏览器以JS的方式去解析文件内容。最后还会会将`css`样式以`style`标签的形式插入到`index.html`的`head`标签里去。

index.css：

```css
.cover{
    border-radius: 10px;
    background-color: red;
}
```



经过`vite`转换之后：

![image-20230108170813939](D:\bicoll-note\WEB前端\Vite\vite知识总结-img\image-20230108170813939.png)

可以发现`css`文件经过转换之后的`JS脚本`默认导出的就是css文件里的内容。



最后在`inde.html`的`head`标签里也可以发现`css`文件里的内容：

![image-20230108171444629](D:\bicoll-note\WEB前端\Vite\vite知识总结-img\image-20230108171444629.png)



#### css模块化




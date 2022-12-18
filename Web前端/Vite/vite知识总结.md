# vite知识总结.md

# 一、

# 二、配置

### 语法提示

### 环境变量

 *// 加载环境变量*

 *// vite内置了dotenv这个第三方库*

 *// dotenv会自动读取.env文件，提取变量注入到`process`下*

 *//检查process.cwd()路径下.env.development.local、.env.development、.env.local、.env这四个环境文件*  

### 不同环境配置

首先为不同的环境创建不同的配置`vite配置文件`。

- vite.dev.config.ts	`开发环境`
- vite.prod.config.ts  `生产环境`
- vite.base.config.ts  `基础配置`
- vite.config.ts  `最终的配置`

`vite.config.ts`

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

### 静态资源处理

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

### 别名






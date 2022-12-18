# Vue3知识总结.md

yarn config set prefix "D:\Programs\node-v16.14.2-win-x64\yarn"

yarn config  set global-folder "D:\Programs\node-v16.14.2-win-x64\yarn\global"

yarn config set cache-folder "D:\Programs\node-v16.14.2-win-x64\yarn\cache"

yarn config set link-folder "D:\Programs\node-v16.14.2-win-x64\yarn\link"

# 一、demo

main.js变化

```js
import {createApp} from 'vue' // 导入createApp函数
import './style.css'
import App from './App.vue'// 导入根组件
// 创建应用实例，挂载到容器#app中
const app = createApp(App)
app.mount('#app')

```

# 二、

1、组合式API

> 特点：一个功能的逻辑集中在一起，包含数据、函数

相比于选项式API，组合式API相关功能代码更加集中，而不是分散在各处，十分便捷，易于维护。
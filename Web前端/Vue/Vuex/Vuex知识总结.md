# Vux知识总结.md

# 一、Vux的基本使用

- 1、安装vux依赖

  ```shell
  npm install vuex --save
  ```

- 2、导入vux包

  ```js
  import Vuex from 'vuex'
  Vue.use(Vuex)
  ```

- 3、创建`store`对象

  ```js
  const store = new Vux.Store({
      // state中存放的就是全局共享数据
      state: { count: 0 }
  })
  ```

  ![image-20220924115424976](D:\bicol-note\Web前端\Vuex\Vuex知识总结-img\image-20220924115424976.png)

![image-20220924115445353](D:\bicol-note\Web前端\Vuex\Vuex知识总结-img\image-20220924115445353.png)
# 二、哔哩哔哩API合集

## 视频信息API

### 视频页面详细信息获取

测试URL：https://api.bilibili.com/x/web-interface/view/detail?bvid=BV1nX4y1P7J2&aid=882914863&need_operation_card=1&web_rm_repeat=&need_elec=1&out_referer=https%3A%2F%2Fwww.bilibili.com%2F

### 视频标签

https://api.bilibili.com/x/web-interface/view/detail/tag?aid=585287138

### 直播推荐

https://api.live.bilibili.com/xlive/web-interface/v1/webMain/getVideoRecList?platform=web&id=36&tid=209

### 视频分页合集，可获取到每个视频的cid

https://api.bilibili.com/x/player/pagelist?bvid=BV1Ez4y1y7EB&jsonp=jsonp

### 视频快照

https://api.bilibili.com/x/player/videoshot?cid=256284790&bvid=BV1Ez4y1y7EB&jsonp=jsonp

### 视频播放地址

- **获取视频的播放地址需要BVID和CID！！！**

- 接口测试，若服务器无反应请忽视该API 

  https://api.bilibili.com/x/player/playurl?otype=json&fnver=0&player=1&fnval=2&qn=64&bvid=BV1Ez4y1y7EB&cid=256284790

- URL

  https://api.bilibili.com/x/player/playurl

- 参数说明

  | 参数     | 参数说明     | 示例         | 是否必须 |
  | -------- | ------------ | ------------ | -------- |
  | otype    | 返回数据类型 | json         | 否       |
  | fnval    | 返回视频格式 | 1            | 否       |
  | **bvid** | 视频的BVID   | BV1Ez4y1y7EB | 是       |
  | **cid**  | 视频的cid    | 256284790    | 是       |
  | qn       | 视频的清晰度 | 64           | 否       |

  **fnval：**参数fnval的值为**1**时返回MP4格式视频链接，值为2时返回flv格式视频链接，值为80时返回m4s格式视频、音频链接，默认返回flv格式视频链接；

  **qn：**

- 精简格式

  https://api.bilibili.com/x/player/playurl?bvid=BV1Ez4y1y7EB&cid=256284790

  此链接已删减为最简短的形式，再简单服务器会报错；

- 返回链接说明

  此接口返回的视频链接具有时效性，超过一定的时间会失效，需再次获取，而且不能直接访问下载，需要在请求头加上以下参数

  | 参数                                                | 说明                                                         |
  | --------------------------------------------------- | ------------------------------------------------------------ |
  | Range:bytes=0-xxx                                   | xxx代表返回数据的数值大小，若想返回整个文件的话填写‘0-’即可； |
  | Referer:https://www.bilibili.com/video/BV1Ez4y1y7EB | 将最末尾的BVID替换成视频对应的BVID即可；                     |



### 获取视频AID，CID（内含视频基本信息，若此BV对应视频属于系列视频，API会列出所有系列视频）：

https://api.bilibili.com/x/web-interface/view?bvid=BV1Ez4y1y7EB

### 相关推荐

- 测试链接：https://api.bilibili.com/x/web-interface/archive/related?aid=716381600&need_operation_card=1&web_rm_repeat=1

## 用户信息API

- https://api.bilibili.com/x/space/arc/search?mid=149040332&pn=1&ps=25&jsonp=jsonp

- 视频推荐API
  - **热搜：**https://s.search.bilibili.com/main/hotword

## 搜索API

- https://api.bilibili.com/x/web-interface/search/all/v2?context=&page=1&order=&keyword=%E6%97%B6%E5%85%89&duration=&tids_1=&tids_2=&from_source=web_search&from_spmid=333.337&platform=pc&__refresh__=true&_extra=&highlight=1&single_column=0

## 推广API

#### **广告：**

https://api.bilibili.com/x/web-show/res/locs?pf=0&ids=2624%2C2625%2C3038%2C4330&bvid=BV1ax411j7yg&aid=16060399

#### **活动：**

https://api.bilibili.com/x/web-interface/archive/related?aid=16060399&need_operation_card=1&web_rm_repeat=1

## B站API分析文章

- **合集 含个人、视频、直播等信息：**

  https://www.bilibili.com/read/cv12357091

- **获取动态：**

  https://blog.csdn.net/h979985773/article/details/104237576?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-13.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-13.control

- **用户信息：**

  https://blog.csdn.net/qq_45587822/article/details/104990670?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162917616516780261966357%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162917616516780261966357&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-6-104990670.first_rank_v2_pc_rank_v29&utm_term=B%E7%AB%99api&spm=1018.2226.3001.4187

- https://www.bilibili.com/read/cv5363590/

- m4s文件：

  https://blog.csdn.net/Enderman_xiaohei/article/details/100598003?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162920587116780261915317%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162920587116780261915317&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-100598003.first_rank_v2_pc_rank_v29&utm_term=m4s&spm=1018.2226.3001.4187


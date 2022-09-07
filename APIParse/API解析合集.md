# API解析合集

## 一、抖音API合集



### 获取短链

https://www.douyin.com/aweme/v1/web/shorten/?targets=https:%2F%2Fwww.iesdouyin.com%2Fshare%2Fvideo%2F6970582109203352839%2F%3Fregion%3DCN%26mid%3D6970582315974036261%26u_code%3Ddk06jb95g0ijkf%26titleType%3Dtitle%26did%3DMS4wLjABAAAAnoqd74tTPRlc5Gov1BnoOpC4J-Zrx2BbqtJy9mMDi_lwuMyWYxaS3QmdGGWrHdUR%26iid%3DMS4wLjABAAAANwkJuWIRFOzg5uCpDRpMj4OX-QryoDgn-yYlXQnRwQQ%26with_sec_did%3D1%26from%3Dweb_code_link&belong=aweme&persist=1&device_platform=webapp&aid=6383&channel=channel_pc_web&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F92.0.4515.131+Safari%2F537.36&browser_online=true&_signature=_02B4Z6wo001012ovEbwAAIDC4WSKXzn3gvNqLxUAALui67

- 参数说明

  | 参数 | 参数说明 | 取值示例 | 是否必须 |
  | ---- | -------- | -------- | -------- |
  | aid  | 未知     | 6383     | 未知     |

  

### 获取评论

https://www.douyin.com/aweme/v1/web/comment/list/?device_platform=webapp&aid=6383&channel=channel_pc_web&aweme_id=6967296128475811084&cursor=0&count=20&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F92.0.4515.131+Safari%2F537.36&browser_online=true&_signature=_02B4Z6wo00901ZMttYQAAIDAGGYuZPyVCtWTLbEAAAXYb8

### 获取视频详细信息

https://www.douyin.com/aweme/v1/web/aweme/detail/?device_platform=webapp&aid=6383&channel=channel_pc_web&aweme_id=6979517533640461609&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F92.0.4515.131+Safari%2F537.36&browser_online=true&_signature=_02B4Z6wo00901x8mGewAAIDClG2CD7OYymMfJh1AAKbf7b

- 请求头信息

  referer:"https://www.douyin.com/video/6965822411929390343?fir_previous_page=main_page&previous_page=video_detail

### 获取相关视频推荐

https://www.douyin.com/aweme/v1/web/aweme/related/?device_platform=webapp&aid=6383&channel=channel_pc_web&aweme_id=6990417163563650334&count=20&filterGids=&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F92.0.4515.131+Safari%2F537.36&browser_online=true&_signature=_02B4Z6wo00d01tlbd0AAAIDDUhDsoeoMNibZW3PAANdX37

### 分析文章

https://blog.csdn.net/ucsheep/article/details/98866480

## 二、哔哩哔哩API合集

### 视频信息API

#### **视频标签**

https://api.bilibili.com/x/web-interface/view/detail/tag?aid=585287138

#### **直播推荐**

https://api.live.bilibili.com/xlive/web-interface/v1/webMain/getVideoRecList?platform=web&id=36&tid=209

#### **视频合集信息详情，可获取到每个视频的cid**

https://api.bilibili.com/x/player/pagelist?bvid=BV1Ez4y1y7EB&jsonp=jsonp

#### **视频快照**

https://api.bilibili.com/x/player/videoshot?cid=256284790&bvid=BV1Ez4y1y7EB&jsonp=jsonp

#### 获取视频播放地址

- **获取视频的播放地址需要BVID和CID！！！**

- 接口测试，若服务器无反应请忽视该API 

  https://api.bilibili.com/x/player/playurl?otype=json&fnver=0&player=1&fnval=1&qn=64&bvid=BV1Ez4y1y7EB&cid=256284790

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

  此接口返回的视频链接不能直接访问下载，需要在请求头加上以下参数：

  | 参数                                                | 说明                                                         |
  | --------------------------------------------------- | ------------------------------------------------------------ |
  | Range:bytes=0-xxx                                   | xxx代表返回数据的数值大小，若想返回整个文件的话填写‘0-’即可； |
  | Referer:https://www.bilibili.com/video/BV1Ez4y1y7EB | 将最末尾的BVID替换成视频对应的BVID即可；                     |



#### **获取视频AID，CID（内含视频基本信息，若此BV对应视频属于系列视频，API会列出所有系列视频）：**

https://api.bilibili.com/x/web-interface/view?bvid=BV1Ez4y1y7EB

### 用户信息API

- https://api.bilibili.com/x/space/arc/search?mid=149040332&pn=1&ps=25&jsonp=jsonp

- 视频推荐API
  - **热搜：**https://s.search.bilibili.com/main/hotword

### 搜索API

- https://api.bilibili.com/x/web-interface/search/all/v2?context=&page=1&order=&keyword=%E6%97%B6%E5%85%89&duration=&tids_1=&tids_2=&from_source=web_search&from_spmid=333.337&platform=pc&__refresh__=true&_extra=&highlight=1&single_column=0

### 推广API

#### **广告：**

https://api.bilibili.com/x/web-show/res/locs?pf=0&ids=2624%2C2625%2C3038%2C4330&bvid=BV1ax411j7yg&aid=16060399

#### **活动：**

https://api.bilibili.com/x/web-interface/archive/related?aid=16060399&need_operation_card=1&web_rm_repeat=1

### B站API分析文章

- **合集 含个人、视频、直播等信息：**

  https://www.bilibili.com/read/cv12357091

- **获取动态：**

  https://blog.csdn.net/h979985773/article/details/104237576?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-13.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-13.control

- **用户信息：**

  https://blog.csdn.net/qq_45587822/article/details/104990670?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162917616516780261966357%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162917616516780261966357&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-6-104990670.first_rank_v2_pc_rank_v29&utm_term=B%E7%AB%99api&spm=1018.2226.3001.4187

- https://www.bilibili.com/read/cv5363590/

- m4s文件：

  https://blog.csdn.net/Enderman_xiaohei/article/details/100598003?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162920587116780261915317%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162920587116780261915317&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-100598003.first_rank_v2_pc_rank_v29&utm_term=m4s&spm=1018.2226.3001.4187
  
  

## 西瓜视频API合集

https://www.ixigua.com/vpc_danmaku/list/v1/?aid=1768&app_name=video_ixigua&format=json&item_id=6979594096016884255&group_id=6979594096016884255&start_time=0&end_time=32000&_signature=_02B4Z6wo00f01XEy.SQAAIDA-nlmx3pjM31xFvmAAD1l0c



### 评论API

https://www.ixigua.com/tlb/comment/article/v5/tab_comments/?tab_index=0&count=10&group_id=6975458085758304804&item_id=6975458085758304804&aid=1768



###  相关推荐

---

referer：https://www.ixigua.com/?wid_try=1

https://www.ixigua.com/api/feedv2/feedById?channelId=94349549020&count=20&queryCount=1&request_from=704&related_gid=6975458085758304804&list_entrance=anyVideo&author_id=106756376612&enable_preloadVideo=1&referrer=https:%2F%2Fwww.ixigua.com%2F%3Fwid_try%3D1&_signature=_02B4Z6wo00f01QD7WowAAIDAi7DBb21uQzEA314AACEV8e



### 视频详情

---

https://www.ixigua.com/api/mixVideo/information?mixId=6975458085758304804&_signature=_02B4Z6wo00f01daUSiQAAIDAXd.Rxi12hGXWsE6AABSKf9

referer:https://www.ixigua.com/6975458085758304804


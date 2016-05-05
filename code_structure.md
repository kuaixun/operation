# 项目结构

#### 总体说明
该部分文档用来描述项目结构、包结构、配置文件描述和模块基本实现逻辑等；目前代码主要分为管理后台、接口服务端、抓取三个项目。
#### 管理后台
SVN地址```svn://192.168.10.154/c-platform/back/glht_svn```

#### 接口服务端
SVN地址```svn://192.168.10.154/c-platform/server/suferDeskTop```

接口服务端是一个maven多模块项目，分为以下模块：

| 模块 | 说明 | 
| -- | -- | 
| common | 通用service、dao、util，主要被suferDeskInteFace和suferDeskExt依赖 |
| suferDeskInteFace | 接口，接口定义参照executeMapper.properties文件，接口文档参照https://kuaixun.gitbooks.io/document/content/， 接口基本实现参照BaseInterServlet类|
| suferDeskExt | 正文抓取和定时器任务，抓取任务参照ExtHandleCallable类，定时任务参照Start类 |
| commonAcrc | 扩展的service、dao、util，主要被sufer-desk-push-control和suferDeskPush依赖 |
| sufer-desk-push-control | 轮询推送 |
| suferDeskPush | 新推送 |
| surfDeskWeatherExt | 天气抓取 |
| surf-desk-t | T+推荐 |
| surfDeskACRC | 错误统计 |
| webp4java | webp图片处理 |
| lottery | 废弃 |
| initData | 废弃 |




#### 抓取

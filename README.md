# tc_zufang
一,项目背景
使用scrapy,redis, mongodb,django实现的一个分布式网络爬虫,底层存储mongodb,分布式使用redis实现,使用django可视化爬虫

这个项目是我学习了pythonde一个项目总结，也是我的一个毕业设计，其中主要是对垂直搜索引擎中分布式网络爬虫的探索实现，暂时它包含一个针对58租房网网站的spider， 可以将其网站所以地方的租房信息的爬取到本地，爬取的项目有标题，租金，地理位置等等项目，此个项目也是作为房源推荐系统的数据采集端，因为本机配置不太好，为了减轻压力，暂时没有加上下载图片pipeline.

二，项目功能与研究内容

1）基于scrapy+redis构建了一个分布式调度队列

将爬虫爬取到的项目详情请求url存储进redis存储队列，上一次请求结束后，再将下一次请求url推出队列给下载器进行爬取，并且多个爬虫可以共享整个请求队列

2）Docker管理分布式环境

更方便的管理爬虫生产环境

3）增量爬取


爬虫可以借助linux的crontab功能进行定时爬取，并且，爬取过的链接，将不会再进行爬取，主要是使用了redis组件的去重策略

4）爬虫防止被ban策略

网站具有一定的防爬措施，主要使用了一下策略来对爬虫进行防爬处理

（1）禁用cookie

（2）设置下载等待时间

（3）使用user_agent池，伪造浏览器信息

（4）使用多个代理ip（暂时没有实现）

5）断点续爬

爬虫遇到异常退出后，能够在上一次中断的队列中继续爬取，其实也使用了redis的一点小策略

6）文件存储

使用单个mongodb存储海量信息

mongodb集群存储（待实现）

7）爬虫调试策略

将系统log信息写到文件中

对重要的log信息(eg:drop item,success)采用彩色样式终端打印（待实现）

8）可视化爬虫数据

使用Django和highcharts


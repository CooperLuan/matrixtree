# 栾艳明 Cooper

+ `Tel` 135 6016 0679
+ `Email` gc.suprs@gmail.com
+ `学历` 华南理工大学 数学系 本科
+ `坐标` 广州 但是广/深/珠/上/杭都 OK
+ `经历` 两年初创公司工作经验

## 项目

### 数据雷达(2012.4 -- now)

>数据雷达是一个淘宝行业数据统计分析/可视化产品, 提供数据收集/统计结果可视化/数据监控功能

+ 基础架构:

    + php(codeigniter) + nginx Web 前端
    + tornado + MySQL + Redis API 服务端
    + python rq 任务处理
    + python etl 数据处理
+ 开发: tornado/MySQL 负责后端/数据库的开发工作
+ 运维: Ubuntu/python/salt-stack/supervisor
+ 重构: codeigniter/tornado 分别做 Web/数据平台API
+ 任务系统: 自开发框架/rq/celery 做任务处理
+ 数据来源/解析: python re/lxml/sasoup(升级后的开发框架)

### 聚客通(2013.5 -- now)

>聚客通是一个统计类客户分析系统,接入用户数据源后可以提供统计分析报表

+ Web: php(codeigniter) nginx
+ 运维: Ubuntu/python/supervisor
+ 任务系统: rq

### sasoup(2014-3)

>sasoup 是一个 python html 解析包, 只需要写好解析配置文件, 传入 html 文本, 即可返回数据字典

+ 基本匹配方式包括 xpath/re.search(正则搜索)/json
+ 在 xpath/re/json 的基础上封装了 next/which/ajax 等复杂匹配方式

    + next 支持从上一次的匹配结果中再次匹配
    + which 定义 n 个匹配表达式直到有匹配结果为止
    + ajax 支持 ajax 数据匹配
+ template 支持模板/继承(super)
+ 更多匹配表达式 base/addon/fields
+ 满足了项目内使用需求, 离标准 python 库还有文档/重构/优化/逻辑覆盖/测试需要完善

## 技术栈

+ python: tornado/Web开发/任务队列/正则/运维
+ 多任务处理: rq
+ linux: ubuntu/开发/初级运维
+ MySQL: 初级优化
+ Web 前端: bootstrap/jQuery/highcharts
+ NoSQL: 基础 redis

## 学习规划

>包括正在学习和打算学习的

+ `flask` 正在学习, 性价比较 tornado 高, 已用于个人工具开发
+ `ElasticSearch` 了解过使用场景/架构, 计划用于个人微博数据索引
+ `PostgreSQL` 暂无计划(用来代替本机 MySQL)
+ `AngularJS` 用来简化个人工具的前端交互部分(翻页...)
+ `SQLAlchemy` 替换自定义 sql 语句
+ `JAVA/C` 有需求时
+ Long Journey

## 一些想法

+ `公司` 希望在技术驱动的公司成长
+ `短期目标` Web 后台开发/最佳实践
+ `方向` Web 开发最佳实践/数据处理/数据库/统计应用

## 标签

+ mac 粉
+ 瘦！但是有规律运动
+ 骑行族
+ 没做过的就不说了..
+ 做的少的也不说了..
+ 谨慎

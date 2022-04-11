
## Introduction
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

Ptubes是一款基于PITR（Point In Time Recovery）方式实现的数据库灾备产品，提供了数据库日志安全备份、高效分发等功能，可用于包括数据备份、数据回放、数据多活等场景。

1.数据订阅

Ptubes将数据库变更数据发送到下游的服务，用户根据自己的需求定制消费逻辑，适用于缓存更新策略、业务异步解耦、异构数据源的数据实时同步和复杂ETL的数据实时同步等多种业务场景。

2.数据同步

Ptubes支持MySQL到MySQL之间的数据实时同步，适用于SET化、数据异地灾备、本地数据灾备、查询与报表分流、实时数据仓库等多种业务场景。

![架构图](https://raw.githubusercontent.com/wiki/meituan/ptubes/images/architecture.png)

## Quick Start

### Prerequisite

* 64bit JDK 1.8+

* Maven 3.2.x

### Run Reader

1.mysql准备, Ptubes同步数据需要提前mysql支持Binlog ROW模式, 需要用户提前修改好binlog模式
```
[mysqld]

log-bin=mysql-bin # 打开 binlog

binlog-format=ROW # 修改为 ROW 模式
```
2.启动Reader
2.1.下载压缩包 --- 后续要补充压缩包位置

2.2.解压到任意目录
```
mkdir /user/ptubes
tar zxvf ptubes-reader-server.tar.gz -C /tmp/ptubes
```
解压完成后可以看到目录结构
```
drwxr-xr-x   4 yangmouren  staff   128  2 17 16:47 bin
drwxr-xr-x   5 yangmouren  staff   160  2 17 16:54 conf
drwxr-xr-x  63 yangmouren  staff  2016  2 17 17:00 lib
```

2.3.修改相关配置信息
在conf目录下，修改一个reader.conf，并填入配置
```ReaderServer.conf
ptubes.server.tasks=demoR1,demoR2 //任务名, 每个任务需要有对应文件的配置信息, 已逗号分隔
```
程序会根据 ptubes.server.tasks 的参数继续读入ReaderTask相关配置。以上图为例，程序会自动从当前目录寻找 demoR1.properties 和 demoR2.properties，我们分别建立对应的文件（如下图所示）
```demoRx.properties
ptubes.reader.mysql.host= //mysql host地址
ptubes.reader.mysql.user= //mysql 用户名
ptubes.reader.mysql.passwd= //mysql密码
```
然后运行程序即可开启ptubes探索之旅。

2.4.启动服务
```
sh bin/start.sh
```
2.5.查看服务日志
```
tail -f logs/reader.log
```
2.6.关闭服务
```
sh bin/stop.sh
```
### 启动SDK
[SDK启动]()
### 更多配置
[配置文档]()

## Documentation
- 1.[Home](https://github.com/meituan/ptubes/wiki)
- 2.[架构设计](https://github.com/meituan/ptubes/wiki/%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1)
    - 2.1.[整体架构与部署](https://github.com/meituan/ptubes/wiki/%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1#1%E6%95%B4%E4%BD%93%E6%9E%B6%E6%9E%84%E4%B8%8E%E9%83%A8%E7%BD%B2)
    - 2.2.[模块说明](https://github.com/meituan/ptubes/wiki/%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1#2%E6%A8%A1%E5%9D%97%E8%AF%B4%E6%98%8E)
    - 2.3.[实现细节](https://github.com/meituan/ptubes/wiki/%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1#3%E5%AE%9E%E7%8E%B0%E7%BB%86%E8%8A%82)
- 3.[QuickStart](https://github.com/meituan/ptubes/wiki/QuickStart)
    - 3.1.[mysql准备](https://github.com/meituan/ptubes/wiki/QuickStart#2mysql%E5%87%86%E5%A4%87)
    - 3.2.[启动Reader](https://github.com/meituan/ptubes/wiki/QuickStart#3%E5%90%AF%E5%8A%A8reader)
- 4.[客户端使用指南](https://github.com/meituan/ptubes/wiki/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97)
    - 4.1.[启动方式一](https://github.com/meituan/ptubes/wiki/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#2%E5%90%AF%E5%8A%A8%E6%96%B9%E5%BC%8F%E4%B8%80)
    - 4.2.[启动方式二](https://github.com/meituan/ptubes/wiki/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#3%E5%90%AF%E5%8A%A8%E6%96%B9%E5%BC%8F%E4%BA%8C)
    - 4.3.[启动方式三](https://github.com/meituan/ptubes/wiki/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#4%E5%90%AF%E5%8A%A8%E6%96%B9%E5%BC%8F%E4%B8%89)
- 5.[配置使用指南](https://github.com/meituan/ptubes/wiki/%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97)
    - 5.1.[Reader配置](https://github.com/meituan/ptubes/wiki/%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#1reader%E9%85%8D%E7%BD%AE)
    - 5.2.[SDK配置](https://github.com/meituan/ptubes/wiki/%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#2sdk%E9%85%8D%E7%BD%AE)
- 6.[本地调试指南](https://github.com/meituan/ptubes/wiki/%E6%9C%AC%E5%9C%B0%E8%B0%83%E8%AF%95%E6%8C%87%E5%8D%97)

## License
[Apache License, Version 2.0](LICENSE) Copyright (C) Apache Software Foundation

## issues
[Ptubes ISSUES](https://github.com/meituan/ptubes/issues)
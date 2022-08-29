---
title: Log4Net日志部署后不输出日志
date: 2022/08/29
tags:
 - Log4Net
categories: 
 - NET
---

> 转载自：[log4net服务启动后没有记录日志](https://www.cnblogs.com/king123/p/11326398.html)

有时候在用log4net的时候，调试或执行是ok的，但是安装服务后没有记录日志。
这是因为服务启动是在C盘启动，而程序放的位置在别的目录。
这时候需要指定读取配置文件的位置为程序所在的目录

原代码
```C#
public static void Configure(string repositoryName ="LogFileAppender", string configFile = "log4net.config")
{
        //控制台应用程序加上以下两句代码
        repository = LogManager.CreateRepository(repositoryName);
        XmlConfigurator.Configure(repository, new FileInfo(configFile));

        _log = LogManager.GetLogger(repositoryName, "Logger");
}
```
更新后

```C#
public static void Configure(string repositoryName ="LogFileAppender", string configFile = "log4net.config")
{
        //获取日志绝对路径
        string execuFilePath = Assembly.GetExecutingAssembly().Location;
        string execuDirPath = Path.GetDirectoryName(execuFilePath);
        string log4netconfigFilePath = execuDirPath + "\\"+ configFile;

        
        //控制台应用程序加上以下两句代码
        repository = LogManager.CreateRepository(repositoryName);
        XmlConfigurator.Configure(repository, new FileInfo(log4netconfigFilePath));

        _log = LogManager.GetLogger(repositoryName, "Logger");
}
```

# 分布式追踪/日志

## 分布式追踪 SkyWalking

## 分布式日志 Exceptionless

### Windows下安装ElasticSearch及工具

> Elasticsearch 是一个分布式、可扩展、实时的搜索与数据分析引擎。 它能从项目一开始就赋予你的数据以搜索、分析和探索的能力，这是通常没有预料到的。 Elasticsearch 不仅仅只是全文搜索，我们还将介绍结构化搜索、数据分析、复杂的语言处理、地理位置和对象间关联关系等。

#### 安装ElasticSearch

* 版本：7.4.0

* [下载地址](https://elasticsearch.cn/download/)

* 解压到本地目录

* 运行bin目录下的elasticsearch.bat文件

* 浏览器访问 http://localhost:9200/ ，如果出现如下则服务启动成功

```json
{
  "name" : "DGCACOAHVD079",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "X5brr9MaRMmcnn3jqlir1w",
  "version" : {
    "number" : "7.4.0",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "22e1767283e61a198cb4db791ea66e3f11ab9910",
    "build_date" : "2019-09-27T08:36:48.569419Z",
    "build_snapshot" : false,
    "lucene_version" : "8.2.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

 **把Elasticsearch安装为Windows服务**

进入bin目录下执行:

```
安装Elasticsearch服务
elasticsearch-service.bat install

删除已安装的Elasticsearch服务（如果启动则停止服务）
elasticsearch-service.bat remove

启动Elasticsearch服务（如果已安装）
elasticsearch-service.bat start

停止服务（如果启动）
elasticsearch-service.bat stop

启动GUI来管理已安装的服务
elasticsearch-service.bat manager
```

#### 安装Kibana

> Kibana 是通向 Elastic 产品集的窗口。 它可以在 Elasticsearch 中对数据进行视觉探索和实时分析。此处简单介绍，稍后会专门写一篇博客介绍Kibana及其用法

* 版本：7.4.0

* [下载地址](https://elasticsearch.cn/download/)

* 解压到本地目录

* 打开 config/kibana.yml，并修改其中elasticsearch.url的值，此处的URL要修改成ElasticSearch启动的URL，默认值为 http://localhost:9200

* 使用 cmd 进入Kibana的bin目录，然后运行kibana.bat文件

* 打开浏览器，访问 http://localhost:5601 即可。如果该端口被占用了，在启动的cmd控制台有显示访问的地址，复制访问即可。

 **把Kibana安装为Windows服务**

 Kibana没有提供安装Windows服务的功能，我们需要借助 nssm.exe 把Kibana安装为Windows服务。

 用命令行执行 nssm.exe install，弹出安装对话框

 * 把 Application Path设置为Kibana安装目录\bin\kibana.bat
 
 * Startup directory设置为安装目录\bin

 * 参数为空
 
 * 服务名称为 Kibana-7.4.0
# 服务注册/发现Consul

## Consul介绍

Consul是一种服务发现和配置工具。Consul具有分布式，高可用性和极高的可扩展性。

Consul提供了几个关键功能：

* 服务发现 - 使用简单的服务来注册自己并通过DNS或HTTP接口发现其他服务。也可以注册SaaS提供商等外部服务。

* 运行状况检查 - 运行状况检查使Consul能够快速向运营商发出有关群集中任何问题的警报 与服务发现的集成可防止将流量路由到不健康的主机，并启用服务级别的断路器。

* 密钥/值存储 - 灵活的密钥/值存储可以存储动态配置，功能标记，协调，领导者选举等。简单的HTTP API使其易于在任何地方使用。

* 多数据中心 - Consul可以识别数据中心，并且可以支持任意数量的区域而无需复杂的配置。

* 服务分段 - Consul Connect通过自动TLS加密和基于身份的授权实现安全的服务到服务通信。

### 快速开始

可以在Consul网站上查看广泛的快速入门：

<https://www.consul.io/intro/getting-started/install.html>

### 文档

可以在Consul网站上查看完整、全面的文档：

<https://www.sonsul.io/docs>

参考资料：

[Consul使用手册](https://blog.csdn.net/liuzhuchen/article/details/81913562)

## Consul使用

创建名称为Consul的Windows服务，并读取D:\Consul\configs文件夹中的配置：

> sc create "Consul" binPath= "D:\Consul\consul.exe agent -config-dir D:\Consul\configs" start= auto

已注册的服务即使关闭了也不会从Consul移除，需要手动移除无效的服务，-id参数后面是服务的ID：

> consul services deregister -id=ServiceId

Consul启动读取配置文件夹会按名称顺序读取，如先读取a.json，然后读取b.json，如果a、b中存在相同的配置项，则 b 会覆盖 a 的值。

Consul服务配置示例：

```json
{
	"datacenter": "ctl1",
	"node_name": "node1",
	"data_dir": "data",
	"performance": {
	  "raft_multiplier": 3
	},
	"server": true,
	"ui": true,
	"ports":{
		"http": 8500,
		"dns": 8600,
		"grpc": 8400,
		"serf_lan": 8301,
		"serf_wan": 8302,
		"server": 8300
	},
	"log_level": "TRACE",
	"log_file": "logs/",
	"bootstrap_expect": 1,
	"client_addr": "0.0.0.0"
}
```
> 注意："client_addr": 默认是127.0.0.1所以不对外提供服务，如果你要对外提供服务改成0.0.0.0。如果是默认127.0.0.1，无法从外部访问Consul的UI界面

从配置文件注册服务：

```json
{
  "services": [
	{
      "id": "service1",
      "name": "ConsulServer",
      "tags": [
        "primary"
      ],
      "address": "localhost",
      "port": 8282,
      "checks": [
        {
        "http": "http://localhost:8282/Server.svc",
        "interval": "10s",
        "timeout": "1s"
        }
      ]
    },
	{
      "id": "service2",
      "name": "ConsulServer",
      "tags": [
        "primary"
      ],
      "address": "localhost",
      "port": 8283,
      "checks": [
        {
        "http": "http://localhost:8283/Server.svc",
        "interval": "10s",
        "timeout": "1s"
        }
      ]
    }
  ]
}
```
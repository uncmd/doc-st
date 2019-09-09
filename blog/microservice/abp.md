# ABP框架与微服务

## 如何获取客户端真实IP，而不是代理IP

自定义实现IClientInfoProvider接口，然后在模块中替换默认的实现

从请求头中获取 X-Forwarded-For 信息，代理会把每层的请求信息存到 X-Forwarded-For 中，包括客户端信息

```csharp
    public class HttpContextClientInfoProxyProvider : HttpContextClientInfoProvider, ITransientDependency
    {
        private readonly IHttpContextAccessor _httpContextAccessor;

        private readonly HttpContext _httpContext;

        public HttpContextClientInfoProxyProvider(IHttpContextAccessor httpContextAccessor)
            : base(httpContextAccessor)
        {
            _httpContextAccessor = httpContextAccessor;
            _httpContext = httpContextAccessor.HttpContext;
        }

        protected override string GetClientIpAddress()
        {
            try
            {
                var httpContext = _httpContextAccessor.HttpContext ?? _httpContext;

                var headers = httpContext?.Request.Headers;
                if (headers != null && headers.ContainsKey("X-Forwarded-For"))
                {
                    httpContext.Connection.RemoteIpAddress = System.Net.IPAddress.Parse(headers["X-Forwarded-For"].ToString().Split(',', StringSplitOptions.RemoveEmptyEntries)[0]);
                }

                return httpContext?.Connection?.RemoteIpAddress?.ToString();
            }
            catch (Exception ex)
            {
                Logger.Warn(ex.ToString());
            }

            return null;
        }
    }
```

在Web.Core模块的预加载事件PreInitialize中替换默认实现：
```csharp
 Configuration.ReplaceService<IClientInfoProvider, HttpContextClientInfoProxyProvider>(DependencyLifeStyle.Transient);
```

> 特别注意：Ocelot网关默认不会存 X-Forwarded-For信息，需要在网关配置文件添加如下配置：

```json
	  "UpstreamHeaderTransform": {
        "X-Forwarded-For": "{RemoteIpAddress}"
	  }
```

这样审计日记获取的客户端IP地址才是真实的。


## 审计日志添加自定义数据（服务端地址）

自己实现IAuditInfoProvider接口，可以继承默认的实现，然后重写Fill方法

```csharp
    public class QmsAuditInfoProvider : DefaultAuditInfoProvider, ITransientDependency
    {
        private readonly IConfigurationRoot _appConfiguration;

        public QmsAuditInfoProvider(IConfigurationRoot configurationRoot)
        {
            _appConfiguration = configurationRoot;
        }

        public override void Fill(AuditInfo auditInfo)
        {
            base.Fill(auditInfo);

            auditInfo.CustomData = $"服务地址：{_appConfiguration["App:ServerRootAddress"]}";
        }
    }
```
	
这里注入了配置文件，从配置文件读取服务地址，然后保存到审计日志的CustomData字段

然后在Web.Host模块的PreInitialize方法中替换IAuditInfoProvider接口的默认实现

```csharp
    Configuration.ReplaceService(typeof(Abp.Auditing.IAuditInfoProvider), () =>
    {
        IocManager.Register<IAuditInfoProvider, AssemblyReportAuditInfoProvider>(DependencyLifeStyle.Transient);
    });
```
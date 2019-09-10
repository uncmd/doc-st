# 持续集成/持续部署Jenkins

## Jenkins介绍

Jenkins是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

[Jenkins官网](https://jenkins.io/zh/)

## Jenkins使用

Jenkin的Execute Shell构建步骤的最后一个命令的退出代码决定了构建步骤的成功/失败
RoboCopy如果所有文件都已成功复制，则返回代码为1. 除了0以外的任何内容都被构建步骤报告为失败
https://stackoverflow.com/questions/33358242/build-step-windows-powershell-marked-build-as-failure-why


部署到IIS上的ASP.NET Core项目，在更新的时候会进程占用的错误
http://m.mamicode.com/info-detail-2709112.html
https://docs.microsoft.com/zh-cn/aspnet/core/host-and-deploy/iis/?view=aspnetcore-2.2#locked-deployment-files


IIS构建配置示例：

dotnet build
dotnet test microservices\AssemblyReport\test\AssemblyReport.Tests\AssemblyReport.Tests.csproj
cd microservices/AssemblyReport/src/AssemblyReport.Web.Host
dotnet publish -c Release -o D:\Jenkins\BuildData\Workspace\Publish\AssemblyReport

rar a -agYYYYMMDDHHMMSS -x*\App_Data D:\QMSService\BackFile\AssemblyReport\Service D:\QMSService\AssemblyReport21031

xcopy D:\QMSService\app_offline.htm D:\QMSService\AssemblyReport21031
robocopy D:\Jenkins\BuildData\Workspace\Publish\AssemblyReport D:\QMSService\AssemblyReport21031 /xf *.json *.config *.pdb /R:1 /W:1
del D:\QMSService\AssemblyReport21031\app_offline.htm
exit 0

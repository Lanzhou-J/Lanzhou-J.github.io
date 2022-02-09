---
title: ASP.Net在docker容器中运行时的ERR_SSL_PROTOCOL_ERROR
layout: post
subtitle: null
date: '2022-1-23 22:39:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- debug
- 中文
---

## 问题描述：

在Mac OS本地Rider IDE运行c# ASP.Net 代码时，Log如下：

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]

```

https://localhost:5001/swagger/index.html

自动打开的Swagger页面也可以正常访问。

但如果使用docker image
```
 docker build -f ./Dockerfile -t "image:latest" "$(pwd)"
 docker run --rm -p 8080:5001 image:latest   
```

结果在运行时会显示：
```
Now listening on: http://[::]:80
```

访问https://localhost:8080/swagger/index.html

结果得到了Error: 

"This page isn’t working -- localhost didn’t send any data."
`ERR_EMPTY_RESPONSE`

or

"This site can’t be reached -- localhost unexpectedly closed the connection."
`ERR_CONNECTION_CLOSED`

使用的Dockerfile如下：
```
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /App

# Copy everything else and build
COPY /ExampleaApp/. .
RUN dotnet publish -c release -o publish

#Build runtime image
FROM mcr.microsoft.com/dotnet/runtime:5.0
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /App

COPY --from=build /App/publish .
EXPOSE 5001
ENTRYPOINT ["dotnet", "ExampleApp.dll"]
```

## 原因分析：

mcr.microsoft.com/dotnet/aspnet 在默认状态下设置`ASPNETCORE_URLS`环境变量为`http://+:80`，如果在没有通过Program.cs中的app.UseUrl在app中设定URL，那么app就会在容器中监听port 80。

尝试把Dockerfile中`EXPOSE 5001`改成`EXPOSE 80`，然后运行如下命令：
```
 docker rmi $(docker image ls -aq)
 docker build -f ./Dockerfile -t "mega-images:latest" "$(pwd)" 
 docker run --rm -p 8080:80 mega-images:latest
```
仍旧访问https://localhost:8080/swagger/index.html

得到以下Error:

“This site can’t provide a secure connection -- localhost sent an invalid response.” 

`ERR_SSL_PROTOCOL_ERROR`

尝试着在本地Terminal运行DockerFile中的`dotnet publish`和`dotnet ExampleApp.dll`命令:
```
dotnet publish -c release -o publish
cd publish
dotnet ExampleApp.dll
```

Log如下：
```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production

```

访问https://localhost:5001/swagger/index.html
得到Error：
"This localhost page can’t be found"
`HTTP ERROR 404`

访问https://localhost:5000/swagger/index.html
得到Error：
"This site can’t provide a secure connection"
`ERR_SSL_PROTOCOL_ERROR`

## 解决方案：

检查了Rider IDE：
Run -> Edit Configurations...

发现如下Environment Variables：
```
ASPNETCORE_URLS=https://localhost:5001;http://localhost:5000 ASPNETCORE_ENVIRONMENT=Development
```
这些环境变量来自于`Mega/Properties/launchSettings.json`(launchSettings.json file already contains the entries for default URLs i.e. http://localhost:5000 & https://localhost:5001 in project settings.)

类似的，可以在本地Mac terminal中设置相同的环境变量：
```
export "ASPNETCORE_URLS=https://localhost:5001;http://localhost:5000"
export "ASPNETCORE_ENVIRONMENT=Development"
```
(HTTPS:5001, HTTP: 5000)

再次运行`dotnet Example.dll`

访问https://localhost:5000/swagger/index.html，得到Error：
`ERR_SSL_PROTOCOL_ERROR`

访问https://localhost:5001/swagger/index.html，Swagger页面可以正常访问。

在Dockerfile中加入：
```
ENV ASPNETCORE_ENVIRONMENT=Development
ENV ASPNETCORE_URLS=http://+:5001;http://+:5000
EXPOSE 5001
EXPOSE 5000
```

运行命令：
```
 docker rmi $(docker image ls -aq)
 docker build -f ./Dockerfile -t "mega-images:latest" "$(pwd)" 
 docker run --rm -p 8080:80 mega-images:latest
```

然后就可以访问Docker container中的ASP.Net app啦：
http://localhost:8080/swagger/index.html

参考资料：
https://stackoverflow.com/questions/48669548/why-does-aspnet-core-start-on-port-80-from-within-docker
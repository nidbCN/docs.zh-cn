---
title: 生成自签名证书概述
description: 简要介绍添加了 .NET Core 和 ASP.NET Core 项目功能的 Microsoft dotnet dev-certs 工具，以及使用自签名证书的其他选项。
author: angee
ms.date: 11/19/2020
ms.openlocfilehash: 738af3fc091e415399a53015a40748ad6116a2b4
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873011"
---
# <a name="generate-self-signed-certificates-with-the-net-cli"></a>通过 .NET CLI 生成自签名证书

使用自签名证书时，可通过不同的方式创建自签名证书，并将它们用于开发和测试场景。  本指南将介绍如何通过 `dotnet dev-certs` 以及 `PowerShell` 和 `OpenSSL` 等其他选项使用自签名证书。

然后，可以使用容器中托管的 [ASP.NET Core 应用](https://github.com/dotnet/dotnet-docker/blob/main/samples/run-aspnetcore-https-development.md)等示例来验证是否将加载证书。

## <a name="prerequisites"></a>先决条件

可在示例中使用 .NET Core 3.1 或 .NET 5。

对于 `dotnet dev-certs`，请确保已安装适当版本的 .NET：

* [在 Windows 上安装 .NET](../install/windows.md)
* [在 Linux 上安装 .NET](../install/linux.md)
* [在 macOS 上安装 .NET](../install/macos.md)

此示例需要 [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) 或更高版本的 [Docker 客户端](https://www.docker.com/products/docker)。

## <a name="prepare-sample-app"></a>准备示例应用

你需要根据要用于测试的运行时（[.NET Core 3.1](#net-core-31-sample-app) 或 [.NET 5](#net-5-sample-app)）来准备示例应用。

对于本指南，你将使用[示例应用](https://hub.docker.com/_/microsoft-dotnet-samples)并进行适当的更改。

### <a name="net-core-31-sample-app"></a>.NET Core 3.1 示例应用

获取示例应用。

```console
git clone https://github.com/dotnet/dotnet-docker/
```

在本地导航到存储库，并在编辑器中打开工作区。

> [!NOTE]
> 如果要使用 dotnet publish 参数对部署进行剪裁，则应确保包含适当的依赖项以便支持 SSL 证书。
更新 [dotnet-docker\samples\aspnetapp\aspnetapp.csproj](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/aspnetapp/aspnetapp.csproj) 以确保容器中包含适当的程序集。 有关参考，请查看如何更新 .csproj 文件以便在对自包含部署应用剪裁时[支持 SSL 证书](../deploying/trim-self-contained.md#support-for-ssl-certificates)。

确保 `aspnetapp.csproj` 包含适当的目标框架：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>.netcoreapp3.1</TargetFramework>
    <!--Other Properties-->
  </PropertyGroup>

</Project>
```

修改 Dockerfile，确保运行时指向 .NET Core 3.1：

```Dockerfile
# https://hub.docker.com/_/microsoft-dotnet-core
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /source

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /source/aspnetapp
RUN dotnet publish -c release -o /app --no-restore

# final stage/image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

确保指向示例应用。

```console
cd .\dotnet-docker\samples\aspnetapp
```

构建用于本地测试的容器。

```console
docker build -t aspnetapp:my-sample -f Dockerfile .
```

### <a name="net-5-sample-app"></a>.NET 5 示例应用

对于本指南，应针对 .NET 5 检查[示例 aspnetapp](https://hub.docker.com/_/microsoft-dotnet-samples)。

检查示例应用 [Dockerfile](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/Dockerfile) 是否使用 .NET 5。

根据主机 OS，可能需要更新 ASP.NET 运行时。 例如，在 Dockerfile 中从 `mcr.microsoft.com/dotnet/aspnet:5.0-nanoservercore-2009 AS runtime` 更改为 `mcr.microsoft.com/dotnet/aspnet:5.0-windowsservercore-ltsc2019 AS runtime` 将有助于面向适当的 Windows 运行时。

例如，这将有助于在 Windows 上测试证书：

```Dockerfile
# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /source

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore -r win-x64

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /source/aspnetapp
RUN dotnet publish -c release -o /app -r win-x64 --self-contained false --no-restore

# final stage/image
# Uses the 2009 release; 2004, 1909, and 1809 are other choices
FROM mcr.microsoft.com/dotnet/aspnet:5.0-windowsservercore-ltsc2019 AS runtime
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["aspnetapp"]

```

如果要在 Linux 上测试证书，可以使用现有的 Dockerfile。

确保 `aspnetapp.csproj` 包含适当的目标框架：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    <!--Other Properties-->
  </PropertyGroup>

</Project>
```

> [!NOTE]
> 如果要使用 `dotnet publish` 参数对部署进行剪裁，请确保包含适当的依赖项以便支持 SSL 证书。
更新 [dotnet-docker\samples\aspnetapp\aspnetapp.csproj](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/aspnetapp/aspnetapp.csproj) 以确保容器中包含适当的程序集。 有关参考，请查看如何更新 .csproj 文件以便在对自包含部署应用剪裁时[支持 SSL 证书](../deploying/trim-self-contained.md#support-for-ssl-certificates)。

确保指向示例应用。

```console
cd .\dotnet-docker\samples\aspnetapp
```

构建用于本地测试的容器。

```console
docker build -t aspnetapp:my-sample -f Dockerfile .
```

## <a name="create-a-self-signed-certificate"></a>创建自签名证书

可以通过以下方法创建自签名证书：

- [使用 dotnet dev-certs](#with-dotnet-dev-certs)
- [使用 PowerShell](#with-powershell)
- [使用 OpenSSL](#with-openssl)

### <a name="with-dotnet-dev-certs"></a>使用 dotnet dev-certs

可以使用 `dotnet dev-certs` 来处理自签名证书。 此示例使用 PowerShell 控制台。

```console
dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\aspnetapp.pfx -p crypticpassword
dotnet dev-certs https --trust
```

> [!NOTE]
> 证书名称（在本例中为 aspnetapp.pfx）必须与项目程序集名称一致。 `crypticpassword` 用作你所选密码的替代对象。 如果控制台返回“已存在有效 HTTPS 证书”，则表示存储中已存在受信任的证书。 可使用 MMC 控制台导出证书。

为证书配置应用程序机密：

```console
dotnet user-secrets -p aspnetapp\aspnetapp.csproj set "Kestrel:Certificates:Development:Password" "crypticpassword"
```

> [!NOTE]
> 注意：密码必须与证书所用的密码一致。

使用为 HTTPS 配置的 ASP.NET Core 运行容器映像：

```console
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -v $env:APPDATA\microsoft\UserSecrets\:C:\Users\ContainerUser\AppData\Roaming\microsoft\UserSecrets -v $env:USERPROFILE\.aspnet\https:C:\Users\ContainerUser\AppData\Roaming\ASP.NET\Https mcr.microsoft.com/dotnet/samples:aspnetapp
```

应用程序启动后，在 Web 浏览器中导航到 `https://localhost:8001`。

#### <a name="clean-up"></a>清理

如果未使用机密和证书，请务必将它们清除。

```console
dotnet user-secrets remove "Kestrel:Certificates:Development:Password" -p aspnetapp\aspnetapp.csproj
dotnet dev-certs https --clean
```

### <a name="with-powershell"></a>使用 PowerShell

可以使用 PowerShell 来生成自签名证书。 可以使用 [PKI 客户端](/powershell/module/pkiclient/new-selfsignedcertificate?preserve-view=true&view=win10-ps)来生成自签名证书。

```powershell
$cert = New-SelfSignedCertificate -DnsName @("contoso.com", "www.contoso.com") -CertStoreLocation "cert:\LocalMachine\My"
```

将生成证书，但出于测试目的，应将证书置于证书存储中，以便在浏览器中进行测试。

```powershell
$certKeyPath = "c:\certs\contoso.com.pfx"
$password = ConvertTo-SecureString 'password' -AsPlainText -Force
$cert | Export-PfxCertificate -FilePath $certKeyPath -Password $password
$rootCert = $(Import-PfxCertificate -FilePath $certKeyPath -CertStoreLocation 'Cert:\LocalMachine\Root' -Password $password)
```

此时，应该可以从 [MMC 管理单元](../../framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in.md)查看证书。

可以在适用于 Linux 的 Windows 子系统 (WSL) 中运行示例容器：

```console
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.pfx -v /c/certs:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

> [!NOTE]
> 请注意，装载卷后，根据主机的不同，文件路径的处理方式可能有所不同。  例如，在 WSL 中，可以将 /c/certs 替换为 /mnt/c/certs 。

如果使用的是之前为 Windows 构建的容器，运行命令将如下所示：

```console
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=c:\https\contoso.com.pfx -v c:\certs:C:\https aspnetapp:my-sample
```

应用程序启动后，在浏览器中导航到 contoso.com:8001。

确保已更新主机条目，以便 `contoso.com` 在适当的 IP 地址（例如 127.0.0.1）上进行应答。 如果无法识别证书，请确保随容器一起加载的证书在主机上也受信任，并确保存在 `contoso.com` 的适当 SAN/DNS 条目。

#### <a name="clean-up"></a>清理

```powershell
$cert | Remove-Item
Get-ChildItem $certFilePath | Remove-Item
$rootCert | Remove-item
```

### <a name="with-openssl"></a>使用 OpenSSL

可以使用 [OpenSSL](https://www.openssl.org/) 来创建自签名证书。 此示例将使用 WSL/Ubuntu 以及带有 `OpenSSL` 的 Bash Shell。

这将生成一个 .crt 和一个 .key 文件。

```bash
PARENT="contoso.com"
openssl req \
-x509 \
-newkey rsa:4096 \
-sha256 \
-days 365 \
-nodes \
-keyout $PARENT.key \
-out $PARENT.crt \
-subj "/CN=${PARENT}" \
-extensions v3_ca \
-extensions v3_req \
-config <( \
  echo '[req]'; \
  echo 'default_bits= 4096'; \
  echo 'distinguished_name=req'; \
  echo 'x509_extension = v3_ca'; \
  echo 'req_extensions = v3_req'; \
  echo '[v3_req]'; \
  echo 'basicConstraints = CA:FALSE'; \
  echo 'keyUsage = nonRepudiation, digitalSignature, keyEncipherment'; \
  echo 'subjectAltName = @alt_names'; \
  echo '[ alt_names ]'; \
  echo "DNS.1 = www.${PARENT}"; \
  echo "DNS.2 = ${PARENT}"; \
  echo '[ v3_ca ]'; \
  echo 'subjectKeyIdentifier=hash'; \
  echo 'authorityKeyIdentifier=keyid:always,issuer'; \
  echo 'basicConstraints = critical, CA:TRUE, pathlen:0'; \
  echo 'keyUsage = critical, cRLSign, keyCertSign'; \
  echo 'extendedKeyUsage = serverAuth, clientAuth')

openssl x509 -noout -text -in $PARENT.crt
```

若要获取 .pfx 文件，请使用以下命令：

```bash
openssl pkcs12 -export -out $PARENT.pfx -inkey $PARENT.key -in $PARENT.crt
```

> [!NOTE]
> .aspnetcore 3.1 示例将使用 `.pfx` 和密码。 从 `.net 5` 运行时开始，Kestrel 还可以采用 `.crt` 和 PEM 编码的 `.key` 文件。

根据主机 OS，需要信任证书。 在 Linux 主机上，“信任”证书有所不同，并且依赖于发行版。

对于本指南，在 Windows 中使用 PowerShell 的示例如下：

```powershell
Import-Certificate -FilePath $certFilePath -CertStoreLocation 'Cert:\LocalMachine\Root'
```

对于 .NET Core 3.1，在 WSL 中运行以下命令：

```bash
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.pfx -v /c/path/to/certs/:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

从 .NET 5 开始，Kestrel 可以采用 `.crt` 和 PEM 编码的 `.key` 文件。 可以通过以下命令针对 .NET 5 运行该示例：

```bash
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.crt -e ASPNETCORE_Kestrel__Certificates__Default__KeyPath=/https/contoso.com.key -v /c/path/to/certs:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

> [!NOTE]
> 请注意，在 WSL 中，卷装载路径可能会根据配置而发生变化。

对于 Windows 中的 .NET Core 3.1，请在 `Powershell` 中运行以下命令：

```powershell
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=c:\https\contoso.com.pfx -v c:\certs:C:\https aspnetapp:my-sample
```

对于 Windows 中的 .NET 5，请在 PowerShell 中运行以下命令：

```powershell
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Path=c:\https\contoso.com.crt -e ASPNETCORE_Kestrel__Certificates__Default__KeyPath=c:\https\contoso.com.key -v c:\certs:C:\https aspnetapp:my-sample
```

应用程序启动后，在浏览器中导航到 contoso.com:8001。

确保已更新主机条目，以便 `contoso.com` 在适当的 IP 地址（例如 127.0.0.1）上进行应答。 如果无法识别证书，请确保随容器一起加载的证书在主机上也受信任，并确保存在 `contoso.com` 的适当 SAN/DNS 条目。

#### <a name="clean-up"></a>清理

确保在完成测试后清除自签名证书。

```powershell
Get-ChildItem $certFilePath | Remove-Item
```

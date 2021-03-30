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
# <a name="generate-self-signed-certificates-with-the-net-cli"></a><span data-ttu-id="13a60-103">通过 .NET CLI 生成自签名证书</span><span class="sxs-lookup"><span data-stu-id="13a60-103">Generate self-signed certificates with the .NET CLI</span></span>

<span data-ttu-id="13a60-104">使用自签名证书时，可通过不同的方式创建自签名证书，并将它们用于开发和测试场景。</span><span class="sxs-lookup"><span data-stu-id="13a60-104">When using self-signed certificates, there are different ways to create and use them for development and testing scenarios.</span></span>  <span data-ttu-id="13a60-105">本指南将介绍如何通过 `dotnet dev-certs` 以及 `PowerShell` 和 `OpenSSL` 等其他选项使用自签名证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-105">In this guide, you'll cover using self-signed certificates with `dotnet dev-certs`, and other options like `PowerShell` and `OpenSSL`.</span></span>

<span data-ttu-id="13a60-106">然后，可以使用容器中托管的 [ASP.NET Core 应用](https://github.com/dotnet/dotnet-docker/blob/main/samples/run-aspnetcore-https-development.md)等示例来验证是否将加载证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-106">You can then validate that the certificate will load using an example such as an [ASP.NET Core app](https://github.com/dotnet/dotnet-docker/blob/main/samples/run-aspnetcore-https-development.md) hosted in a container.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13a60-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="13a60-107">Prerequisites</span></span>

<span data-ttu-id="13a60-108">可在示例中使用 .NET Core 3.1 或 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="13a60-108">In the sample, you can utilize either .NET Core 3.1 or .NET 5.</span></span>

<span data-ttu-id="13a60-109">对于 `dotnet dev-certs`，请确保已安装适当版本的 .NET：</span><span class="sxs-lookup"><span data-stu-id="13a60-109">For `dotnet dev-certs`, be sure to have the appropriate version of .NET installed:</span></span>

* [<span data-ttu-id="13a60-110">在 Windows 上安装 .NET</span><span class="sxs-lookup"><span data-stu-id="13a60-110">Install .NET on Windows</span></span>](../install/windows.md)
* [<span data-ttu-id="13a60-111">在 Linux 上安装 .NET</span><span class="sxs-lookup"><span data-stu-id="13a60-111">Install .NET on Linux</span></span>](../install/linux.md)
* [<span data-ttu-id="13a60-112">在 macOS 上安装 .NET</span><span class="sxs-lookup"><span data-stu-id="13a60-112">Install .NET on macOS</span></span>](../install/macos.md)

<span data-ttu-id="13a60-113">此示例需要 [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) 或更高版本的 [Docker 客户端](https://www.docker.com/products/docker)。</span><span class="sxs-lookup"><span data-stu-id="13a60-113">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prepare-sample-app"></a><span data-ttu-id="13a60-114">准备示例应用</span><span class="sxs-lookup"><span data-stu-id="13a60-114">Prepare sample app</span></span>

<span data-ttu-id="13a60-115">你需要根据要用于测试的运行时（[.NET Core 3.1](#net-core-31-sample-app) 或 [.NET 5](#net-5-sample-app)）来准备示例应用。</span><span class="sxs-lookup"><span data-stu-id="13a60-115">You'll need to prepare the sample app depending on which runtime you'd like to use for testing, either [.NET Core 3.1](#net-core-31-sample-app) or [.NET 5](#net-5-sample-app).</span></span>

<span data-ttu-id="13a60-116">对于本指南，你将使用[示例应用](https://hub.docker.com/_/microsoft-dotnet-samples)并进行适当的更改。</span><span class="sxs-lookup"><span data-stu-id="13a60-116">For this guide, you'll use a [sample app](https://hub.docker.com/_/microsoft-dotnet-samples) and make changes where appropriate.</span></span>

### <a name="net-core-31-sample-app"></a><span data-ttu-id="13a60-117">.NET Core 3.1 示例应用</span><span class="sxs-lookup"><span data-stu-id="13a60-117">.NET Core 3.1 sample app</span></span>

<span data-ttu-id="13a60-118">获取示例应用。</span><span class="sxs-lookup"><span data-stu-id="13a60-118">Get the sample app.</span></span>

```console
git clone https://github.com/dotnet/dotnet-docker/
```

<span data-ttu-id="13a60-119">在本地导航到存储库，并在编辑器中打开工作区。</span><span class="sxs-lookup"><span data-stu-id="13a60-119">Navigate to the repository locally and open up the workspace in an editor.</span></span>

> [!NOTE]
> <span data-ttu-id="13a60-120">如果要使用 dotnet publish 参数对部署进行剪裁，则应确保包含适当的依赖项以便支持 SSL 证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-120">If you're looking to use dotnet publish parameters to *trim* the deployment, you should make sure that the appropriate dependencies are included for supporting SSL certificates.</span></span>
<span data-ttu-id="13a60-121">更新 [dotnet-docker\samples\aspnetapp\aspnetapp.csproj](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/aspnetapp/aspnetapp.csproj) 以确保容器中包含适当的程序集。</span><span class="sxs-lookup"><span data-stu-id="13a60-121">Update the [dotnet-docker\samples\aspnetapp\aspnetapp.csproj](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/aspnetapp/aspnetapp.csproj) to ensure that the appropriate assemblies are included in the container.</span></span> <span data-ttu-id="13a60-122">有关参考，请查看如何更新 .csproj 文件以便在对自包含部署应用剪裁时[支持 SSL 证书](../deploying/trim-self-contained.md#support-for-ssl-certificates)。</span><span class="sxs-lookup"><span data-stu-id="13a60-122">For reference, check how to update the .csproj file to [support ssl certificates](../deploying/trim-self-contained.md#support-for-ssl-certificates) when using trimming for self-contained deployments.</span></span>

<span data-ttu-id="13a60-123">确保 `aspnetapp.csproj` 包含适当的目标框架：</span><span class="sxs-lookup"><span data-stu-id="13a60-123">Make sure the `aspnetapp.csproj` includes the appropriate target framework:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>.netcoreapp3.1</TargetFramework>
    <!--Other Properties-->
  </PropertyGroup>

</Project>
```

<span data-ttu-id="13a60-124">修改 Dockerfile，确保运行时指向 .NET Core 3.1：</span><span class="sxs-lookup"><span data-stu-id="13a60-124">Modify the Dockerfile to make sure the runtime points to .NET Core 3.1:</span></span>

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

<span data-ttu-id="13a60-125">确保指向示例应用。</span><span class="sxs-lookup"><span data-stu-id="13a60-125">Make sure you're pointing to the sample app.</span></span>

```console
cd .\dotnet-docker\samples\aspnetapp
```

<span data-ttu-id="13a60-126">构建用于本地测试的容器。</span><span class="sxs-lookup"><span data-stu-id="13a60-126">Build the container for testing locally.</span></span>

```console
docker build -t aspnetapp:my-sample -f Dockerfile .
```

### <a name="net-5-sample-app"></a><span data-ttu-id="13a60-127">.NET 5 示例应用</span><span class="sxs-lookup"><span data-stu-id="13a60-127">.NET 5 sample app</span></span>

<span data-ttu-id="13a60-128">对于本指南，应针对 .NET 5 检查[示例 aspnetapp](https://hub.docker.com/_/microsoft-dotnet-samples)。</span><span class="sxs-lookup"><span data-stu-id="13a60-128">For this guide, the [sample aspnetapp](https://hub.docker.com/_/microsoft-dotnet-samples) should be checked for .NET 5.</span></span>

<span data-ttu-id="13a60-129">检查示例应用 [Dockerfile](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/Dockerfile) 是否使用 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="13a60-129">Check sample app [Dockerfile](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/Dockerfile) is using .NET 5.</span></span>

<span data-ttu-id="13a60-130">根据主机 OS，可能需要更新 ASP.NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="13a60-130">Depending on the host OS, the ASP.NET runtime may need to be updated.</span></span> <span data-ttu-id="13a60-131">例如，在 Dockerfile 中从 `mcr.microsoft.com/dotnet/aspnet:5.0-nanoservercore-2009 AS runtime` 更改为 `mcr.microsoft.com/dotnet/aspnet:5.0-windowsservercore-ltsc2019 AS runtime` 将有助于面向适当的 Windows 运行时。</span><span class="sxs-lookup"><span data-stu-id="13a60-131">For example, changing from `mcr.microsoft.com/dotnet/aspnet:5.0-nanoservercore-2009 AS runtime` to `mcr.microsoft.com/dotnet/aspnet:5.0-windowsservercore-ltsc2019 AS runtime` in the Dockerfile will help with targeting the appropriate Windows runtime.</span></span>

<span data-ttu-id="13a60-132">例如，这将有助于在 Windows 上测试证书：</span><span class="sxs-lookup"><span data-stu-id="13a60-132">For example, this will help with testing the certificates on Windows:</span></span>

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

<span data-ttu-id="13a60-133">如果要在 Linux 上测试证书，可以使用现有的 Dockerfile。</span><span class="sxs-lookup"><span data-stu-id="13a60-133">If we're testing the certificates on Linux, you can use the existing Dockerfile.</span></span>

<span data-ttu-id="13a60-134">确保 `aspnetapp.csproj` 包含适当的目标框架：</span><span class="sxs-lookup"><span data-stu-id="13a60-134">Make sure the `aspnetapp.csproj` includes the appropriate target framework:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    <!--Other Properties-->
  </PropertyGroup>

</Project>
```

> [!NOTE]
> <span data-ttu-id="13a60-135">如果要使用 `dotnet publish` 参数对部署进行剪裁，请确保包含适当的依赖项以便支持 SSL 证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-135">If you want to use `dotnet publish` parameters to *trim* the deployment, make sure that the appropriate dependencies are included for supporting SSL certificates.</span></span>
<span data-ttu-id="13a60-136">更新 [dotnet-docker\samples\aspnetapp\aspnetapp.csproj](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/aspnetapp/aspnetapp.csproj) 以确保容器中包含适当的程序集。</span><span class="sxs-lookup"><span data-stu-id="13a60-136">Update the [dotnet-docker\samples\aspnetapp\aspnetapp.csproj](https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/aspnetapp/aspnetapp.csproj) to ensure that the appropriate assemblies are included in the container.</span></span> <span data-ttu-id="13a60-137">有关参考，请查看如何更新 .csproj 文件以便在对自包含部署应用剪裁时[支持 SSL 证书](../deploying/trim-self-contained.md#support-for-ssl-certificates)。</span><span class="sxs-lookup"><span data-stu-id="13a60-137">For reference, check how to update the .csproj file to [support ssl certificates](../deploying/trim-self-contained.md#support-for-ssl-certificates) when using trimming for self-contained deployments.</span></span>

<span data-ttu-id="13a60-138">确保指向示例应用。</span><span class="sxs-lookup"><span data-stu-id="13a60-138">Make sure you're pointing to the sample app.</span></span>

```console
cd .\dotnet-docker\samples\aspnetapp
```

<span data-ttu-id="13a60-139">构建用于本地测试的容器。</span><span class="sxs-lookup"><span data-stu-id="13a60-139">Build the container for testing locally.</span></span>

```console
docker build -t aspnetapp:my-sample -f Dockerfile .
```

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="13a60-140">创建自签名证书</span><span class="sxs-lookup"><span data-stu-id="13a60-140">Create a self-signed certificate</span></span>

<span data-ttu-id="13a60-141">可以通过以下方法创建自签名证书：</span><span class="sxs-lookup"><span data-stu-id="13a60-141">You can create a self-signed certificate:</span></span>

- [<span data-ttu-id="13a60-142">使用 dotnet dev-certs</span><span class="sxs-lookup"><span data-stu-id="13a60-142">With dotnet dev-certs</span></span>](#with-dotnet-dev-certs)
- [<span data-ttu-id="13a60-143">使用 PowerShell</span><span class="sxs-lookup"><span data-stu-id="13a60-143">With PowerShell</span></span>](#with-powershell)
- [<span data-ttu-id="13a60-144">使用 OpenSSL</span><span class="sxs-lookup"><span data-stu-id="13a60-144">With OpenSSL</span></span>](#with-openssl)

### <a name="with-dotnet-dev-certs"></a><span data-ttu-id="13a60-145">使用 dotnet dev-certs</span><span class="sxs-lookup"><span data-stu-id="13a60-145">With dotnet dev-certs</span></span>

<span data-ttu-id="13a60-146">可以使用 `dotnet dev-certs` 来处理自签名证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-146">You can use `dotnet dev-certs` to work with self-signed certificates.</span></span> <span data-ttu-id="13a60-147">此示例使用 PowerShell 控制台。</span><span class="sxs-lookup"><span data-stu-id="13a60-147">This example uses a PowerShell console.</span></span>

```console
dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\aspnetapp.pfx -p crypticpassword
dotnet dev-certs https --trust
```

> [!NOTE]
> <span data-ttu-id="13a60-148">证书名称（在本例中为 aspnetapp.pfx）必须与项目程序集名称一致。</span><span class="sxs-lookup"><span data-stu-id="13a60-148">The certificate name, in this case *aspnetapp*.pfx must match the project assembly name.</span></span> <span data-ttu-id="13a60-149">`crypticpassword` 用作你所选密码的替代对象。</span><span class="sxs-lookup"><span data-stu-id="13a60-149">`crypticpassword` is used as a stand-in for a password of your own choosing.</span></span> <span data-ttu-id="13a60-150">如果控制台返回“已存在有效 HTTPS 证书”，则表示存储中已存在受信任的证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-150">If console returns "A valid HTTPS certificate is already present.", a trusted certificate already exists in your store.</span></span> <span data-ttu-id="13a60-151">可使用 MMC 控制台导出证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-151">It can be exported using MMC Console.</span></span>

<span data-ttu-id="13a60-152">为证书配置应用程序机密：</span><span class="sxs-lookup"><span data-stu-id="13a60-152">Configure application secrets, for the certificate:</span></span>

```console
dotnet user-secrets -p aspnetapp\aspnetapp.csproj set "Kestrel:Certificates:Development:Password" "crypticpassword"
```

> [!NOTE]
> <span data-ttu-id="13a60-153">注意：密码必须与证书所用的密码一致。</span><span class="sxs-lookup"><span data-stu-id="13a60-153">Note: The password must match the password used for the certificate.</span></span>

<span data-ttu-id="13a60-154">使用为 HTTPS 配置的 ASP.NET Core 运行容器映像：</span><span class="sxs-lookup"><span data-stu-id="13a60-154">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -v $env:APPDATA\microsoft\UserSecrets\:C:\Users\ContainerUser\AppData\Roaming\microsoft\UserSecrets -v $env:USERPROFILE\.aspnet\https:C:\Users\ContainerUser\AppData\Roaming\ASP.NET\Https mcr.microsoft.com/dotnet/samples:aspnetapp
```

<span data-ttu-id="13a60-155">应用程序启动后，在 Web 浏览器中导航到 `https://localhost:8001`。</span><span class="sxs-lookup"><span data-stu-id="13a60-155">Once the application starts, navigate to `https://localhost:8001` in your web browser.</span></span>

#### <a name="clean-up"></a><span data-ttu-id="13a60-156">清理</span><span class="sxs-lookup"><span data-stu-id="13a60-156">Clean up</span></span>

<span data-ttu-id="13a60-157">如果未使用机密和证书，请务必将它们清除。</span><span class="sxs-lookup"><span data-stu-id="13a60-157">If the secrets and certificates are not in use, be sure to clean them up.</span></span>

```console
dotnet user-secrets remove "Kestrel:Certificates:Development:Password" -p aspnetapp\aspnetapp.csproj
dotnet dev-certs https --clean
```

### <a name="with-powershell"></a><span data-ttu-id="13a60-158">使用 PowerShell</span><span class="sxs-lookup"><span data-stu-id="13a60-158">With PowerShell</span></span>

<span data-ttu-id="13a60-159">可以使用 PowerShell 来生成自签名证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-159">You can use PowerShell to generate self-signed certificates.</span></span> <span data-ttu-id="13a60-160">可以使用 [PKI 客户端](/powershell/module/pkiclient/new-selfsignedcertificate?preserve-view=true&view=win10-ps)来生成自签名证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-160">The [PKI Client](/powershell/module/pkiclient/new-selfsignedcertificate?preserve-view=true&view=win10-ps) can be used to generate a self-signed certificate.</span></span>

```powershell
$cert = New-SelfSignedCertificate -DnsName @("contoso.com", "www.contoso.com") -CertStoreLocation "cert:\LocalMachine\My"
```

<span data-ttu-id="13a60-161">将生成证书，但出于测试目的，应将证书置于证书存储中，以便在浏览器中进行测试。</span><span class="sxs-lookup"><span data-stu-id="13a60-161">The certificate will be generated, but for the purposes of testing, should be placed in a cert store for testing in a browser.</span></span>

```powershell
$certKeyPath = "c:\certs\contoso.com.pfx"
$password = ConvertTo-SecureString 'password' -AsPlainText -Force
$cert | Export-PfxCertificate -FilePath $certKeyPath -Password $password
$rootCert = $(Import-PfxCertificate -FilePath $certKeyPath -CertStoreLocation 'Cert:\LocalMachine\Root' -Password $password)
```

<span data-ttu-id="13a60-162">此时，应该可以从 [MMC 管理单元](../../framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in.md)查看证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-162">At this point, the certificates should be viewable from an [MMC snap-in](../../framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in.md).</span></span>

<span data-ttu-id="13a60-163">可以在适用于 Linux 的 Windows 子系统 (WSL) 中运行示例容器：</span><span class="sxs-lookup"><span data-stu-id="13a60-163">You can run the sample container in Windows Subsystem for Linux (WSL):</span></span>

```console
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.pfx -v /c/certs:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

> [!NOTE]
> <span data-ttu-id="13a60-164">请注意，装载卷后，根据主机的不同，文件路径的处理方式可能有所不同。</span><span class="sxs-lookup"><span data-stu-id="13a60-164">Note that with the volume mount the file path could be handled differently based on host.</span></span>  <span data-ttu-id="13a60-165">例如，在 WSL 中，可以将 /c/certs 替换为 /mnt/c/certs 。</span><span class="sxs-lookup"><span data-stu-id="13a60-165">For example, in WSL we may replace */c/certs* with */mnt/c/certs*.</span></span>

<span data-ttu-id="13a60-166">如果使用的是之前为 Windows 构建的容器，运行命令将如下所示：</span><span class="sxs-lookup"><span data-stu-id="13a60-166">If you're using the container built earlier for Windows, the run command would look like the following:</span></span>

```console
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=c:\https\contoso.com.pfx -v c:\certs:C:\https aspnetapp:my-sample
```

<span data-ttu-id="13a60-167">应用程序启动后，在浏览器中导航到 contoso.com:8001。</span><span class="sxs-lookup"><span data-stu-id="13a60-167">Once the application is up, navigate to contoso.com:8001 in a browser.</span></span>

<span data-ttu-id="13a60-168">确保已更新主机条目，以便 `contoso.com` 在适当的 IP 地址（例如 127.0.0.1）上进行应答。</span><span class="sxs-lookup"><span data-stu-id="13a60-168">Be sure that the host entries are updated for `contoso.com` to answer on  the appropriate ip address (for example 127.0.0.1).</span></span> <span data-ttu-id="13a60-169">如果无法识别证书，请确保随容器一起加载的证书在主机上也受信任，并确保存在 `contoso.com` 的适当 SAN/DNS 条目。</span><span class="sxs-lookup"><span data-stu-id="13a60-169">If the certificate isn't recognized, make sure that the certificate that is loaded with the container is also trusted on the host, and that there's appropriate SAN / DNS entries for `contoso.com`.</span></span>

#### <a name="clean-up"></a><span data-ttu-id="13a60-170">清理</span><span class="sxs-lookup"><span data-stu-id="13a60-170">Clean up</span></span>

```powershell
$cert | Remove-Item
Get-ChildItem $certFilePath | Remove-Item
$rootCert | Remove-item
```

### <a name="with-openssl"></a><span data-ttu-id="13a60-171">使用 OpenSSL</span><span class="sxs-lookup"><span data-stu-id="13a60-171">With OpenSSL</span></span>

<span data-ttu-id="13a60-172">可以使用 [OpenSSL](https://www.openssl.org/) 来创建自签名证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-172">You can use [OpenSSL](https://www.openssl.org/) to create self-signed certificates.</span></span> <span data-ttu-id="13a60-173">此示例将使用 WSL/Ubuntu 以及带有 `OpenSSL` 的 Bash Shell。</span><span class="sxs-lookup"><span data-stu-id="13a60-173">This example will use WSL / Ubuntu and a bash shell with `OpenSSL`.</span></span>

<span data-ttu-id="13a60-174">这将生成一个 .crt 和一个 .key 文件。</span><span class="sxs-lookup"><span data-stu-id="13a60-174">This will generate a .crt and a .key.</span></span>

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

<span data-ttu-id="13a60-175">若要获取 .pfx 文件，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="13a60-175">To get a .pfx, use the following command:</span></span>

```bash
openssl pkcs12 -export -out $PARENT.pfx -inkey $PARENT.key -in $PARENT.crt
```

> [!NOTE]
> <span data-ttu-id="13a60-176">.aspnetcore 3.1 示例将使用 `.pfx` 和密码。</span><span class="sxs-lookup"><span data-stu-id="13a60-176">The .aspnetcore 3.1 example will use `.pfx` and a password.</span></span> <span data-ttu-id="13a60-177">从 `.net 5` 运行时开始，Kestrel 还可以采用 `.crt` 和 PEM 编码的 `.key` 文件。</span><span class="sxs-lookup"><span data-stu-id="13a60-177">Starting with the `.net 5` runtime, Kestrel can also take `.crt` and PEM-encoded `.key` files.</span></span>

<span data-ttu-id="13a60-178">根据主机 OS，需要信任证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-178">Depending on the host os, the certificate will need to be trusted.</span></span> <span data-ttu-id="13a60-179">在 Linux 主机上，“信任”证书有所不同，并且依赖于发行版。</span><span class="sxs-lookup"><span data-stu-id="13a60-179">On a Linux host, 'trusting' the certificate is different and distro dependent.</span></span>

<span data-ttu-id="13a60-180">对于本指南，在 Windows 中使用 PowerShell 的示例如下：</span><span class="sxs-lookup"><span data-stu-id="13a60-180">For the purposes of this guide, here's an example in Windows using PowerShell:</span></span>

```powershell
Import-Certificate -FilePath $certFilePath -CertStoreLocation 'Cert:\LocalMachine\Root'
```

<span data-ttu-id="13a60-181">对于 .NET Core 3.1，在 WSL 中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="13a60-181">For .NET Core 3.1, run the following command in WSL:</span></span>

```bash
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.pfx -v /c/path/to/certs/:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

<span data-ttu-id="13a60-182">从 .NET 5 开始，Kestrel 可以采用 `.crt` 和 PEM 编码的 `.key` 文件。</span><span class="sxs-lookup"><span data-stu-id="13a60-182">Starting with .NET 5, Kestrel can take the `.crt` and PEM-encoded `.key` files.</span></span> <span data-ttu-id="13a60-183">可以通过以下命令针对 .NET 5 运行该示例：</span><span class="sxs-lookup"><span data-stu-id="13a60-183">You can run the sample with the following command for .NET 5:</span></span>

```bash
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.crt -e ASPNETCORE_Kestrel__Certificates__Default__KeyPath=/https/contoso.com.key -v /c/path/to/certs:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

> [!NOTE]
> <span data-ttu-id="13a60-184">请注意，在 WSL 中，卷装载路径可能会根据配置而发生变化。</span><span class="sxs-lookup"><span data-stu-id="13a60-184">Note that in WSL, the volume mount path may change depending on the configuration.</span></span>

<span data-ttu-id="13a60-185">对于 Windows 中的 .NET Core 3.1，请在 `Powershell` 中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="13a60-185">For .NET Core 3.1 in Windows, run the following command in `Powershell`:</span></span>

```powershell
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=c:\https\contoso.com.pfx -v c:\certs:C:\https aspnetapp:my-sample
```

<span data-ttu-id="13a60-186">对于 Windows 中的 .NET 5，请在 PowerShell 中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="13a60-186">For .NET 5 in Windows, run the following command in PowerShell:</span></span>

```powershell
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Path=c:\https\contoso.com.crt -e ASPNETCORE_Kestrel__Certificates__Default__KeyPath=c:\https\contoso.com.key -v c:\certs:C:\https aspnetapp:my-sample
```

<span data-ttu-id="13a60-187">应用程序启动后，在浏览器中导航到 contoso.com:8001。</span><span class="sxs-lookup"><span data-stu-id="13a60-187">Once the application is up, navigate to contoso.com:8001 in a browser.</span></span>

<span data-ttu-id="13a60-188">确保已更新主机条目，以便 `contoso.com` 在适当的 IP 地址（例如 127.0.0.1）上进行应答。</span><span class="sxs-lookup"><span data-stu-id="13a60-188">Be sure that the host entries are updated for `contoso.com` to answer on  the appropriate ip address (for example 127.0.0.1).</span></span> <span data-ttu-id="13a60-189">如果无法识别证书，请确保随容器一起加载的证书在主机上也受信任，并确保存在 `contoso.com` 的适当 SAN/DNS 条目。</span><span class="sxs-lookup"><span data-stu-id="13a60-189">If the certificate isn't recognized, make sure that the certificate that is loaded with the container is also trusted on the host, and that there's appropriate SAN / DNS entries for `contoso.com`.</span></span>

#### <a name="clean-up"></a><span data-ttu-id="13a60-190">清理</span><span class="sxs-lookup"><span data-stu-id="13a60-190">Clean up</span></span>

<span data-ttu-id="13a60-191">确保在完成测试后清除自签名证书。</span><span class="sxs-lookup"><span data-stu-id="13a60-191">Be sure to clean up the self-signed certificates once done testing.</span></span>

```powershell
Get-ChildItem $certFilePath | Remove-Item
```

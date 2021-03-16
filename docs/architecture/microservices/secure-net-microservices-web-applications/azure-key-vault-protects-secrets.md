---
title: 在生产时使用 Azure Key Vault 保护机密
description: .NET 微服务和 Web 应用程序中的安全性 - Azure Key Vault 是处理完全由管理员控制的应用程序机密的绝佳方式。 管理员甚至可以在不需要开发人员处理的情况下分配和撤销开发值。
author: mjrousos
ms.date: 01/30/2020
ms.openlocfilehash: 94243468b35e2db9818ae9afacd5beec0a12075e
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604614"
---
# <a name="use-azure-key-vault-to-protect-secrets-at-production-time"></a><span data-ttu-id="68689-104">在生产时使用 Azure Key Vault 保护机密</span><span class="sxs-lookup"><span data-stu-id="68689-104">Use Azure Key Vault to protect secrets at production time</span></span>

<span data-ttu-id="68689-105">存储为环境变量的机密或由机密管理器工具存储的机密仍在本地存储并在计算机上解密。</span><span class="sxs-lookup"><span data-stu-id="68689-105">Secrets stored as environment variables or stored by the Secret Manager tool are still stored locally and unencrypted on the machine.</span></span> <span data-ttu-id="68689-106">用于存储机密更加安全的选项是使用 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)，它为存储密钥和机密提供安全的中央位置。</span><span class="sxs-lookup"><span data-stu-id="68689-106">A more secure option for storing secrets is [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), which provides a secure, central location for storing keys and secrets.</span></span>

<span data-ttu-id="68689-107">Azure.Extensions.AspNetCore.Configuration.Secrets 包允许 ASP.NET Core 应用程序从 Azure Key Vault 中读取配置信息。</span><span class="sxs-lookup"><span data-stu-id="68689-107">The **Azure.Extensions.AspNetCore.Configuration.Secrets** package allows an ASP.NET Core application to read configuration information from Azure Key Vault.</span></span> <span data-ttu-id="68689-108">若要开始通过 Azure Key Vault 使用机密，请按照下列步骤操作：</span><span class="sxs-lookup"><span data-stu-id="68689-108">To start using secrets from an Azure Key Vault, you follow these steps:</span></span>

1. <span data-ttu-id="68689-109">将应用程序注册为 Azure AD 应用程序。</span><span class="sxs-lookup"><span data-stu-id="68689-109">Register your application as an Azure AD application.</span></span> <span data-ttu-id="68689-110">（对密钥保管库的访问由 Azure AD 管理。）可以通过 Azure 管理门户完成此操作。</span><span class="sxs-lookup"><span data-stu-id="68689-110">(Access to key vaults is managed by Azure AD.) This can be done through the Azure management portal.</span></span>\

   <span data-ttu-id="68689-111">或者，如果希望应用程序使用证书而不是密码或客户端密码进行身份验证，可以使用 [New-AzADApplication](/powershell/module/az.resources/new-azadapplication) PowerShell cmdlet。</span><span class="sxs-lookup"><span data-stu-id="68689-111">Alternatively, if you want your application to authenticate using a certificate instead of a password or client secret, you can use the [New-AzADApplication](/powershell/module/az.resources/new-azadapplication) PowerShell cmdlet.</span></span> <span data-ttu-id="68689-112">向 Azure Key Vault 注册的证书仅需要公钥。</span><span class="sxs-lookup"><span data-stu-id="68689-112">The certificate that you register with Azure Key Vault needs only your public key.</span></span> <span data-ttu-id="68689-113">应用程序将使用私钥。</span><span class="sxs-lookup"><span data-stu-id="68689-113">Your application will use the private key.</span></span>

2. <span data-ttu-id="68689-114">通过创建新的服务主体授予已注册应用程序访问密钥保管库的权限。</span><span class="sxs-lookup"><span data-stu-id="68689-114">Give the registered application access to the key vault by creating a new service principal.</span></span> <span data-ttu-id="68689-115">可以使用以下 PowerShell 命令来执行此操作：</span><span class="sxs-lookup"><span data-stu-id="68689-115">You can do this using the following PowerShell commands:</span></span>

   ```powershell
   $sp = New-AzADServicePrincipal -ApplicationId "<Application ID guid>"
   Set-AzKeyVaultAccessPolicy -VaultName "<VaultName>" -ServicePrincipalName $sp.ServicePrincipalNames[0] -PermissionsToSecrets all -ResourceGroupName "<KeyVault Resource Group>"
   ```

3. <span data-ttu-id="68689-116">创建 <xref:Microsoft.Extensions.Configuration.IConfigurationRoot> 实例时通过调用 AzureKeyVaultConfigurationExtensions.AddAzureKeyVault 扩展方法将密钥保管库作为配置源包括在应用程序中。</span><span class="sxs-lookup"><span data-stu-id="68689-116">Include the key vault as a configuration source in your application by calling the AzureKeyVaultConfigurationExtensions.AddAzureKeyVault extension method when you create an <xref:Microsoft.Extensions.Configuration.IConfigurationRoot> instance.</span></span>

<span data-ttu-id="68689-117">请注意，调用 `AddAzureKeyVault` 将需要前面步骤中已注册并已获取密钥保管库访问权限的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="68689-117">Note that calling `AddAzureKeyVault` requires the application ID that was registered and given access to the key vault in the previous steps.</span></span> <span data-ttu-id="68689-118">或者，可以首先运行 Azure CLI 命令 `az login`，然后使用 `AddAzureKeyVault` 重载，该重载使用 DefaultAzureCredential 来替代客户端。</span><span class="sxs-lookup"><span data-stu-id="68689-118">Or you can firstly running the Azure CLI command: `az login`, then using an overload of `AddAzureKeyVault` that takes a DefaultAzureCredential in place of the client.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68689-119">建议将 Azure Key Vault 注册为最后一个配置提供程序，这样它就可以覆盖之前提供程序的配置值。</span><span class="sxs-lookup"><span data-stu-id="68689-119">We recommend that you register Azure Key Vault as the last configuration provider, so it can override configuration values from previous providers.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68689-120">其他资源</span><span class="sxs-lookup"><span data-stu-id="68689-120">Additional resources</span></span>

- <span data-ttu-id="68689-121">**使用 Azure Key Vault 保护应用程序机密** </span><span class="sxs-lookup"><span data-stu-id="68689-121">**Using Azure Key Vault to protect application secrets** </span></span>\
  [https://docs.microsoft.com/azure/architecture/multitenant-identity](/azure/architecture/multitenant-identity)

- <span data-ttu-id="68689-122">**在开发期间安全存储应用机密** </span><span class="sxs-lookup"><span data-stu-id="68689-122">**Safe storage of app secrets during development** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/app-secrets](/aspnet/core/security/app-secrets)

- <span data-ttu-id="68689-123">**配置数据保护** </span><span class="sxs-lookup"><span data-stu-id="68689-123">**Configuring data protection** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview](/aspnet/core/security/data-protection/configuration/overview)

- <span data-ttu-id="68689-124">**ASP.NET Core 中的数据保护密钥管理和生存期信息** </span><span class="sxs-lookup"><span data-stu-id="68689-124">**Data Protection key management and lifetime in ASP.NET Core** </span></span>\
  [https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/default-settings](/aspnet/core/security/data-protection/configuration/default-settings)

>[!div class="step-by-step"]
><span data-ttu-id="68689-125">[上一页](developer-app-secrets-storage.md)
>[下一页](../key-takeaways.md)</span><span class="sxs-lookup"><span data-stu-id="68689-125">[Previous](developer-app-secrets-storage.md)
[Next](../key-takeaways.md)</span></span>

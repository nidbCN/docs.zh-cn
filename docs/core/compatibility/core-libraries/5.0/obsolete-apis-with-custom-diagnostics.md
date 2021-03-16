---
title: 中断性变更：API 已过时并带有非默认诊断 ID
description: 了解核心 .NET 库中的 .NET 5 中断性变更：某些 API 使用自定义诊断 ID 被标记了为已过时。
ms.date: 11/01/2020
ms.openlocfilehash: 9aa4543ae6660f2d2fceac2419340bc6c90f1c54
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257370"
---
# <a name="api-obsoletions-with-non-default-diagnostic-ids"></a><span data-ttu-id="6ae76-103">API 已过时并带有非默认诊断 ID</span><span class="sxs-lookup"><span data-stu-id="6ae76-103">API obsoletions with non-default diagnostic IDs</span></span>

<span data-ttu-id="6ae76-104">从 .NET 5.0 开始，某些 API 标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="6ae76-104">Some APIs have been marked as obsolete, starting in .NET 5.0.</span></span> <span data-ttu-id="6ae76-105">此中断性变更特定于标记为已过时并带有自定义诊断 ID 的 API。</span><span class="sxs-lookup"><span data-stu-id="6ae76-105">This breaking change is specific to APIs that have been marked as obsolete *with a custom diagnostic ID*.</span></span> <span data-ttu-id="6ae76-106">取消默认过时诊断 ID（对于 C# 编译器，即为 [CS0618](../../../../csharp/language-reference/compiler-messages/cs0618.md)），不会取消使用这些 API 时编译器生成的警告。</span><span class="sxs-lookup"><span data-stu-id="6ae76-106">Suppressing the default obsoletion diagnostic ID, which is [CS0618](../../../../csharp/language-reference/compiler-messages/cs0618.md) for the C# compiler, does not suppress the warnings that the compiler generates when these APIs are used.</span></span>

## <a name="change-description"></a><span data-ttu-id="6ae76-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="6ae76-107">Change description</span></span>

<span data-ttu-id="6ae76-108">在以前的 .NET 版本中，可以使用这些 API，而不会引发任何生成警告。</span><span class="sxs-lookup"><span data-stu-id="6ae76-108">In previous .NET versions, these APIs can be used without any build warning.</span></span> <span data-ttu-id="6ae76-109">在 .NET 5 及更高版本中，使用这些 API 会生成编译时警告或错误，并显示自定义诊断 ID。</span><span class="sxs-lookup"><span data-stu-id="6ae76-109">In .NET 5 and later versions, use of these APIS produces a compile-time warning or error with a custom diagnostic ID.</span></span> <span data-ttu-id="6ae76-110">使用自定义诊断 ID 可单独取消某个过时警告，而不是取消所有过时警告。</span><span class="sxs-lookup"><span data-stu-id="6ae76-110">The use of custom diagnostic IDs allows you to suppress the obsoletion warnings individually instead of blanket-suppressing all obsoletion warnings.</span></span>

<span data-ttu-id="6ae76-111">下表列出了已过时 API 的自定义诊断 ID 及其相应的警告消息。</span><span class="sxs-lookup"><span data-stu-id="6ae76-111">The following table lists the custom diagnostic IDs and their corresponding warning messages for obsoleted APIs.</span></span>

| <span data-ttu-id="6ae76-112">诊断 ID</span><span class="sxs-lookup"><span data-stu-id="6ae76-112">Diagnostic ID</span></span> | <span data-ttu-id="6ae76-113">说明</span><span class="sxs-lookup"><span data-stu-id="6ae76-113">Description</span></span> | <span data-ttu-id="6ae76-114">严重性</span><span class="sxs-lookup"><span data-stu-id="6ae76-114">Severity</span></span> |
| - | - |
| `SYSLIB0001` | <span data-ttu-id="6ae76-115">UTF-7 编码不安全，因此不应使用。</span><span class="sxs-lookup"><span data-stu-id="6ae76-115">The UTF-7 encoding is insecure and should not be used.</span></span> <span data-ttu-id="6ae76-116">请考虑改用 UTF-8。</span><span class="sxs-lookup"><span data-stu-id="6ae76-116">Consider using UTF-8 instead.</span></span> | <span data-ttu-id="6ae76-117">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-117">Warning</span></span> |
| `SYSLIB0002` | <span data-ttu-id="6ae76-118"><xref:System.Security.Permissions.PrincipalPermissionAttribute> 不受运行时支持，不得使用。</span><span class="sxs-lookup"><span data-stu-id="6ae76-118"><xref:System.Security.Permissions.PrincipalPermissionAttribute> is not honored by the runtime and must not be used.</span></span> | <span data-ttu-id="6ae76-119">错误</span><span class="sxs-lookup"><span data-stu-id="6ae76-119">Error</span></span> |
| `SYSLIB0003` | <span data-ttu-id="6ae76-120">运行时不支持或不接受代码访问安全性 (CAS)。</span><span class="sxs-lookup"><span data-stu-id="6ae76-120">Code access security (CAS) is not supported or honored by the runtime.</span></span> | <span data-ttu-id="6ae76-121">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-121">Warning</span></span> |
| `SYSLIB0004` | <span data-ttu-id="6ae76-122">不支持受约束的执行区域 (CER) 功能。</span><span class="sxs-lookup"><span data-stu-id="6ae76-122">The constrained execution region (CER) feature is not supported.</span></span> | <span data-ttu-id="6ae76-123">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-123">Warning</span></span> |
| `SYSLIB0005` | <span data-ttu-id="6ae76-124">不支持全局程序集缓存 (GAC)。</span><span class="sxs-lookup"><span data-stu-id="6ae76-124">The global assembly cache (GAC) is not supported.</span></span> | <span data-ttu-id="6ae76-125">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-125">Warning</span></span> |
| `SYSLIB0006` | <span data-ttu-id="6ae76-126"><xref:System.Threading.Thread.Abort?displayProperty=nameWithType> 不受支持并会引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="6ae76-126"><xref:System.Threading.Thread.Abort?displayProperty=nameWithType> is not supported and throws <xref:System.PlatformNotSupportedException>.</span></span> | <span data-ttu-id="6ae76-127">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-127">Warning</span></span> |
| `SYSLIB0007` | <span data-ttu-id="6ae76-128">不支持此加密算法的默认实现。</span><span class="sxs-lookup"><span data-stu-id="6ae76-128">The default implementation of this cryptography algorithm is not supported.</span></span> | <span data-ttu-id="6ae76-129">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-129">Warning</span></span> |
| `SYSLIB0008` | <span data-ttu-id="6ae76-130"><xref:System.Runtime.CompilerServices.DebugInfoGenerator.CreatePdbGenerator> API 不受支持并会引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="6ae76-130">The <xref:System.Runtime.CompilerServices.DebugInfoGenerator.CreatePdbGenerator> API is not supported and throws <xref:System.PlatformNotSupportedException>.</span></span> | <span data-ttu-id="6ae76-131">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-131">Warning</span></span> |
| `SYSLIB0009` | <span data-ttu-id="6ae76-132"><xref:System.Net.AuthenticationManager.Authenticate%2A?displayProperty=nameWithType> 和 <xref:System.Net.AuthenticationManager.PreAuthenticate%2A?displayProperty=nameWithType> 方法都不受支持并会引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="6ae76-132">The <xref:System.Net.AuthenticationManager.Authenticate%2A?displayProperty=nameWithType> and <xref:System.Net.AuthenticationManager.PreAuthenticate%2A?displayProperty=nameWithType> methods are not supported and throw <xref:System.PlatformNotSupportedException>.</span></span> | <span data-ttu-id="6ae76-133">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-133">Warning</span></span> |
| `SYSLIB0010`| <span data-ttu-id="6ae76-134">某些远程处理 API 不受支持并会引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="6ae76-134">Some remoting APIs are not supported and throw <xref:System.PlatformNotSupportedException>.</span></span> | <span data-ttu-id="6ae76-135">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-135">Warning</span></span> |
| `SYSLIB0011` | <span data-ttu-id="6ae76-136"><xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 序列化已过时，不应使用。</span><span class="sxs-lookup"><span data-stu-id="6ae76-136"><xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> serialization is obsolete and should not be used.</span></span> | <span data-ttu-id="6ae76-137">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-137">Warning</span></span> |
| `SYSLIB0012` | <span data-ttu-id="6ae76-138">包含 <xref:System.Reflection.Assembly.CodeBase?displayProperty=nameWithType> 和 <xref:System.Reflection.Assembly.EscapedCodeBase?displayProperty=nameWithType> 只是为了实现 .NET Framework 兼容性。</span><span class="sxs-lookup"><span data-stu-id="6ae76-138"><xref:System.Reflection.Assembly.CodeBase?displayProperty=nameWithType> and <xref:System.Reflection.Assembly.EscapedCodeBase?displayProperty=nameWithType> are only included for .NET Framework compatibility.</span></span> <span data-ttu-id="6ae76-139">请改用 <xref:System.Reflection.Assembly.Location?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="6ae76-139">Use <xref:System.Reflection.Assembly.Location?displayProperty=nameWithType> instead.</span></span> | <span data-ttu-id="6ae76-140">警告</span><span class="sxs-lookup"><span data-stu-id="6ae76-140">Warning</span></span> |

## <a name="version-introduced"></a><span data-ttu-id="6ae76-141">引入的版本</span><span class="sxs-lookup"><span data-stu-id="6ae76-141">Version introduced</span></span>

<span data-ttu-id="6ae76-142">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="6ae76-142">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="6ae76-143">建议操作</span><span class="sxs-lookup"><span data-stu-id="6ae76-143">Recommended action</span></span>

- <span data-ttu-id="6ae76-144">使用警告中提供的 URL 链接，遵循为每个诊断 ID 提供的特定指南。</span><span class="sxs-lookup"><span data-stu-id="6ae76-144">Follow the specific guidance provided for the each diagnostic ID using the URL link provided on the warning.</span></span>

- <span data-ttu-id="6ae76-145">不能使用过时类型或成员的标准诊断 ID 取消有关这些过时类型或成员的警告或错误；请改用自定义 `SYSLIBxxxx` 诊断 ID 值。</span><span class="sxs-lookup"><span data-stu-id="6ae76-145">Warnings or errors for these obsoletions can't be suppressed using the standard diagnostic ID for obsolete types or members; use the custom `SYSLIBxxxx` diagnostic ID value instead.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="6ae76-146">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="6ae76-146">Affected APIs</span></span>

### <a name="syslib0001"></a><span data-ttu-id="6ae76-147">SYSLIB0001</span><span class="sxs-lookup"><span data-stu-id="6ae76-147">SYSLIB0001</span></span>

- <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType>
- <xref:System.Text.UTF7Encoding.%23ctor%2A>

### <a name="syslib0002"></a><span data-ttu-id="6ae76-148">SYSLIB0002</span><span class="sxs-lookup"><span data-stu-id="6ae76-148">SYSLIB0002</span></span>

- <xref:System.Security.Permissions.PrincipalPermissionAttribute.%23ctor(System.Security.Permissions.SecurityAction)>

### <a name="syslib0003"></a><span data-ttu-id="6ae76-149">SYSLIB0003</span><span class="sxs-lookup"><span data-stu-id="6ae76-149">SYSLIB0003</span></span>

<span data-ttu-id="6ae76-150">`System.Security.Permissions` 命名空间中的类：</span><span class="sxs-lookup"><span data-stu-id="6ae76-150">Classes in the `System.Security.Permissions` namespace:</span></span>

- <xref:System.Security.Permissions.CodeAccessSecurityAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.DataProtectionPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.DataProtectionPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.EnvironmentPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.EnvironmentPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.FileDialogPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.FileDialogPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.FileIOPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.FileIOPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.GacIdentityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.GacIdentityPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.HostProtectionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.IsolatedStorageFilePermission?displayProperty=fullName>
- <xref:System.Security.Permissions.IsolatedStorageFilePermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.IsolatedStoragePermission?displayProperty=fullName>
- <xref:System.Security.Permissions.IsolatedStoragePermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.KeyContainerPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.KeyContainerPermissionAccessEntry?displayProperty=fullName>
- <xref:System.Security.Permissions.KeyContainerPermissionAccessEntryCollection?displayProperty=fullName>
- <xref:System.Security.Permissions.KeyContainerPermissionAccessEntryEnumerator?displayProperty=fullName>
- <xref:System.Security.Permissions.KeyContainerPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.MediaPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.MediaPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.PermissionSetAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.PrincipalPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.PrincipalPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.PublisherIdentityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.PublisherIdentityPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.ReflectionPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.ReflectionPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.RegistryPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.RegistryPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.ResourcePermissionBase?displayProperty=fullName>
- <xref:System.Security.Permissions.ResourcePermissionBaseEntry?displayProperty=fullName>
- <xref:System.Security.Permissions.SecurityAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.SecurityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.SecurityPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.SiteIdentityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.SiteIdentityPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.StorePermission?displayProperty=fullName>
- <xref:System.Security.Permissions.StorePermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.StrongNameIdentityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.StrongNameIdentityPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.StrongNamePublicKeyBlob?displayProperty=fullName>
- <xref:System.Security.Permissions.TypeDescriptorPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.TypeDescriptorPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.UIPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.UIPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.UrlIdentityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.UrlIdentityPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.WebBrowserPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.WebBrowserPermissionAttribute?displayProperty=fullName>
- <xref:System.Security.Permissions.ZoneIdentityPermission?displayProperty=fullName>
- <xref:System.Security.Permissions.ZoneIdentityPermissionAttribute?displayProperty=fullName>

<span data-ttu-id="6ae76-151">派生自 `CodeAccessSecurityAttribute` 的类：</span><span class="sxs-lookup"><span data-stu-id="6ae76-151">Classes that derive from `CodeAccessSecurityAttribute`:</span></span>

- <xref:System.Configuration.ConfigurationPermissionAttribute?displayProperty=fullName>
- <xref:System.Data.Common.DBDataPermissionAttribute?displayProperty=fullName>
- <xref:System.Data.Odbc.OdbcPermissionAttribute?displayProperty=fullName>
- <xref:System.Data.OleDb.OleDbPermissionAttribute?displayProperty=fullName>
- <xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=fullName>
- <xref:System.Data.SqlClient.SqlClientPermissionAttribute?displayProperty=fullName>
- <xref:System.Diagnostics.EventLogPermissionAttribute?displayProperty=fullName>
- <xref:System.Diagnostics.PerformanceCounterPermissionAttribute?displayProperty=fullName>
- <xref:System.DirectoryServices.DirectoryServicesPermissionAttribute?displayProperty=fullName>
- <xref:System.Drawing.Printing.PrintingPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.DnsPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.SocketPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.WebPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.Mail.SmtpPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.NetworkInformation.NetworkInformationPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.PeerToPeer.PnrpPermissionAttribute?displayProperty=fullName>
- <xref:System.Net.PeerToPeer.Collaboration.PeerCollaborationPermissionAttribute?displayProperty=fullName>
- <xref:System.ServiceProcess.ServiceControllerPermissionAttribute?displayProperty=fullName>
- <xref:System.Transactions.DistributedTransactionPermissionAttribute?displayProperty=fullName>
- <xref:System.Web.AspNetHostingPermissionAttribute?displayProperty=fullName>

<span data-ttu-id="6ae76-152">接口：</span><span class="sxs-lookup"><span data-stu-id="6ae76-152">Interfaces:</span></span>

- <xref:System.Security.Permissions.IUnrestrictedPermission?displayProperty=fullName>
- <xref:System.Security.IPermission?displayProperty=fullName>
- <xref:System.Security.IStackWalk?displayProperty=fullName>
- <xref:System.Security.Policy.IIdentityPermissionFactory?displayProperty=fullName>

<span data-ttu-id="6ae76-153">实现 `IStackWalk` 的类：</span><span class="sxs-lookup"><span data-stu-id="6ae76-153">Classes that implement `IStackWalk`:</span></span>

- <xref:System.Security.NamedPermissionSet?displayProperty=fullName>
- <xref:System.Security.PermissionSet?displayProperty=fullName>

<span data-ttu-id="6ae76-154">实现 `IPermission` 的类：</span><span class="sxs-lookup"><span data-stu-id="6ae76-154">Classes that implement `IPermission`:</span></span>

- <xref:System.Security.CodeAccessPermission?displayProperty=fullName>

<span data-ttu-id="6ae76-155">派生自 `CodeAccessPermission` 的类：</span><span class="sxs-lookup"><span data-stu-id="6ae76-155">Classes that derive from `CodeAccessPermission`:</span></span>

- <xref:System.Configuration.ConfigurationPermission?displayProperty=fullName>
- <xref:System.Data.Common.DBDataPermission?displayProperty=fullName>
- <xref:System.Data.Odbc.OdbcPermission?displayProperty=fullName>
- <xref:System.Data.OleDb.OleDbPermission?displayProperty=fullName>
- <xref:System.Data.SqlClient.SqlClientPermission?displayProperty=fullName>
- <xref:System.Data.OracleClient.OraclePermission?displayProperty=fullName>
- <xref:System.Drawing.Printing.PrintingPermission?displayProperty=fullName>
- <xref:System.Net.DnsPermission?displayProperty=fullName>
- <xref:System.Net.SocketPermission?displayProperty=fullName>
- <xref:System.Net.WebPermission?displayProperty=fullName>
- <xref:System.Net.Mail.SmtpPermission?displayProperty=fullName>
- <xref:System.Net.NetworkInformation.NetworkInformationPermission?displayProperty=fullName>
- <xref:System.Net.PeerToPeer.PnrpPermission?displayProperty=fullName>
- <xref:System.Net.PeerToPeer.Collaboration.PeerCollaborationPermission?displayProperty=fullName>
- <xref:System.Transactions.DistributedTransactionPermission?displayProperty=fullName>
- <xref:System.Web.AspNetHostingPermission?displayProperty=fullName>
- <xref:System.Xaml.Permissions.XamlLoadPermission?displayProperty=fullName>

<span data-ttu-id="6ae76-156">派生自 `ResourcePermissionBase` 的类：</span><span class="sxs-lookup"><span data-stu-id="6ae76-156">Classes that derive from `ResourcePermissionBase`:</span></span>

- <xref:System.Diagnostics.EventLogPermission?displayProperty=fullName>
- <xref:System.Diagnostics.PerformanceCounterPermission?displayProperty=fullName>
- <xref:System.DirectoryServices.DirectoryServicesPermission?displayProperty=fullName>
- <xref:System.ServiceProcess.ServiceControllerPermission?displayProperty=fullName>

<span data-ttu-id="6ae76-157">`System.Security.Permissions` 命名空间中的枚举：</span><span class="sxs-lookup"><span data-stu-id="6ae76-157">Enums in the `System.Security.Permissions` namespace:</span></span>

- <xref:System.Security.Permissions.DataProtectionPermissionFlags?displayProperty=fullName>
- <xref:System.Security.Permissions.EnvironmentPermissionAccess?displayProperty=fullName>
- <xref:System.Security.Permissions.FileDialogPermissionAccess?displayProperty=fullName>
- <xref:System.Security.Permissions.FileIOPermissionAccess?displayProperty=fullName>
- <xref:System.Security.Permissions.HostProtectionResource?displayProperty=fullName>
- <xref:System.Security.Permissions.IsolatedStorageContainment?displayProperty=fullName>
- <xref:System.Security.Permissions.KeyContainerPermissionFlags?displayProperty=fullName>
- <xref:System.Security.Permissions.MediaPermissionAudio?displayProperty=fullName>
- <xref:System.Security.Permissions.MediaPermissionImage?displayProperty=fullName>
- <xref:System.Security.Permissions.MediaPermissionVideo?displayProperty=fullName>
- <xref:System.Security.Permissions.PermissionState?displayProperty=fullName>
- <xref:System.Security.Permissions.ReflectionPermissionFlag?displayProperty=fullName>
- <xref:System.Security.Permissions.RegistryPermissionAccess?displayProperty=fullName>
- <xref:System.Security.Permissions.SecurityAction?displayProperty=fullName>
- <xref:System.Security.Permissions.SecurityPermissionFlag?displayProperty=fullName>
- <xref:System.Security.Permissions.StorePermissionFlags?displayProperty=fullName>
- <xref:System.Security.Permissions.TypeDescriptorPermissionFlags?displayProperty=fullName>
- <xref:System.Security.Permissions.UIPermissionClipboard?displayProperty=fullName>
- <xref:System.Security.Permissions.UIPermissionWindow?displayProperty=fullName>
- <xref:System.Security.Permissions.WebBrowserPermissionLevel?displayProperty=fullName>

<span data-ttu-id="6ae76-158">依赖于代码访问安全类型的类和成员：</span><span class="sxs-lookup"><span data-stu-id="6ae76-158">Classes and members that depend on code access security types:</span></span>

- <xref:System.AppDomain.PermissionSet?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.AllowReversePInvokeCallsAttribute?displayProperty=fullName>
- <xref:System.Security.HostProtectionException?displayProperty=fullName>
- <xref:System.Security.Policy.FileCodeGroup?displayProperty=fullName>
- <xref:System.Security.Policy.StrongName?displayProperty=fullName>
- <xref:System.Security.Policy.StrongNameMembershipCondition?displayProperty=fullName>
- [<span data-ttu-id="6ae76-159">System.Security.Policy.ApplicationTrust.ApplicationTrust(PermissionSet, IEnumerable\<StrongName>)</span><span class="sxs-lookup"><span data-stu-id="6ae76-159">System.Security.Policy.ApplicationTrust.ApplicationTrust(PermissionSet, IEnumerable\<StrongName>)</span></span>](/dotnet/api/system.security.policy.applicationtrust.-ctor#System_Security_Policy_ApplicationTrust__ctor_System_Security_PermissionSet_System_Collections_Generic_IEnumerable_System_Security_Policy_StrongName__)
- <xref:System.Security.Policy.ApplicationTrust.FullTrustAssemblies?displayProperty=fullName>
- <xref:System.Security.Policy.GacInstalled?displayProperty=fullName>
- <xref:System.Security.Policy.PolicyStatement.%23ctor%2A?displayProperty=fullName>
- <xref:System.Security.Policy.PolicyLevel.AddNamedPermissionSet(System.Security.NamedPermissionSet)?displayProperty=fullName>
- <xref:System.Security.Policy.PolicyLevel.ChangeNamedPermissionSet(System.String,System.Security.PermissionSet)?displayProperty=fullName>
- <xref:System.Security.Policy.PolicyLevel.GetNamedPermissionSet(System.String)?displayProperty=fullName>
- <xref:System.Security.Policy.PolicyLevel.RemoveNamedPermissionSet(System.String)?displayProperty=fullName>
- <xref:System.Security.Policy.PolicyLevel.RemoveNamedPermissionSet(System.Security.NamedPermissionSet)?displayProperty=nameWithType>
- <xref:System.Security.Policy.PolicyStatement.PermissionSet?displayProperty=fullName>
- <xref:System.Security.Policy.Publisher?displayProperty=fullName>
- <xref:System.Security.Policy.Site?displayProperty=fullName>
- <xref:System.Security.Policy.Url?displayProperty=fullName>
- <xref:System.Security.Policy.Zone?displayProperty=fullName>
- <xref:System.Security.SecurityManager?displayProperty=fullName>

### <a name="syslib0004"></a><span data-ttu-id="6ae76-160">SYSLIB0004</span><span class="sxs-lookup"><span data-stu-id="6ae76-160">SYSLIB0004</span></span>

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup(System.Runtime.CompilerServices.RuntimeHelpers.TryCode,System.Runtime.CompilerServices.RuntimeHelpers.CleanupCode,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions?displayProperty=nameWithType>
- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegionsNoOP?displayProperty=nameWithType>
- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareContractedDelegate(System.Delegate)?displayProperty=nameWithType>
- <xref:System.Runtime.CompilerServices.RuntimeHelpers.ProbeForSufficientStack?displayProperty=nameWithType>
- <xref:System.Runtime.ConstrainedExecution.Cer?displayProperty=nameWithType>
- <xref:System.Runtime.ConstrainedExecution.Consistency?displayProperty=nameWithType>
- <xref:System.Runtime.ConstrainedExecution.PrePrepareMethodAttribute?displayProperty=nameWithType>
- <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute?displayProperty=nameWithType>

### <a name="syslib0005"></a><span data-ttu-id="6ae76-161">SYSLIB0005</span><span class="sxs-lookup"><span data-stu-id="6ae76-161">SYSLIB0005</span></span>

- <xref:System.Reflection.Assembly.GlobalAssemblyCache?displayProperty=nameWithType>

### <a name="syslib0006"></a><span data-ttu-id="6ae76-162">SYSLIB0006</span><span class="sxs-lookup"><span data-stu-id="6ae76-162">SYSLIB0006</span></span>

- <xref:System.Threading.Thread.Abort?displayProperty=nameWithType>
- <xref:System.Threading.Thread.Abort(System.Object)?displayProperty=nameWithType>

### <a name="syslib0007"></a><span data-ttu-id="6ae76-163">SYSLIB0007</span><span class="sxs-lookup"><span data-stu-id="6ae76-163">SYSLIB0007</span></span>

- <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.HMAC.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Create?displayProperty=fullName>

### <a name="syslib0008"></a><span data-ttu-id="6ae76-164">SYSLIB0008</span><span class="sxs-lookup"><span data-stu-id="6ae76-164">SYSLIB0008</span></span>

- <xref:System.Runtime.CompilerServices.DebugInfoGenerator.CreatePdbGenerator?displayProperty=nameWithType>

### <a name="syslib0009"></a><span data-ttu-id="6ae76-165">SYSLIB0009</span><span class="sxs-lookup"><span data-stu-id="6ae76-165">SYSLIB0009</span></span>

- <xref:System.Net.AuthenticationManager.Authenticate%2A?displayProperty=nameWithType>
- <xref:System.Net.AuthenticationManager.PreAuthenticate%2A?displayProperty=nameWithType>

### <a name="syslib0010"></a><span data-ttu-id="6ae76-166">SYSLIB0010</span><span class="sxs-lookup"><span data-stu-id="6ae76-166">SYSLIB0010</span></span>

- <xref:System.MarshalByRefObject.GetLifetimeService?displayProperty=nameWithType>
- <xref:System.MarshalByRefObject.InitializeLifetimeService?displayProperty=nameWithType>

### <a name="syslib0011"></a><span data-ttu-id="6ae76-167">SYSLIB0011</span><span class="sxs-lookup"><span data-stu-id="6ae76-167">SYSLIB0011</span></span>

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

### <a name="syslib0012"></a><span data-ttu-id="6ae76-168">SYSLIB0012</span><span class="sxs-lookup"><span data-stu-id="6ae76-168">SYSLIB0012</span></span>

- <xref:System.Reflection.Assembly.CodeBase?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.EscapedCodeBase?displayProperty=nameWithType>

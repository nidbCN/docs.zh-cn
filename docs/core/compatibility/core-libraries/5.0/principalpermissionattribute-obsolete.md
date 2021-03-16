---
title: 中断性变更：PrincipalPermissionAttribute 已过时，报告为错误
description: 了解核心 .NET 库中的 .NET 5 中断性变更：PrincipalPermissionAttribute 构造函数已过时，会生成编译时错误。
ms.date: 11/01/2020
ms.openlocfilehash: 7568883935633e98b884b553efccf50504448b77
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257227"
---
# <a name="principalpermissionattribute-is-obsolete-as-error"></a><span data-ttu-id="1524c-103">PrincipalPermissionAttribute 已过时，报告为错误</span><span class="sxs-lookup"><span data-stu-id="1524c-103">PrincipalPermissionAttribute is obsolete as error</span></span>

<span data-ttu-id="1524c-104"><xref:System.Security.Permissions.PrincipalPermissionAttribute> 构造函数已过时，会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1524c-104">The <xref:System.Security.Permissions.PrincipalPermissionAttribute> constructor is obsolete and produces a compile-time error.</span></span> <span data-ttu-id="1524c-105">不能实例化此属性或将其应用于方法。</span><span class="sxs-lookup"><span data-stu-id="1524c-105">You cannot instantiate this attribute or apply it to a method.</span></span>

## <a name="change-description"></a><span data-ttu-id="1524c-106">更改描述</span><span class="sxs-lookup"><span data-stu-id="1524c-106">Change description</span></span>

<span data-ttu-id="1524c-107">在 .NET Framework 和 .NET Core 上，可以用 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 属性批注方法。</span><span class="sxs-lookup"><span data-stu-id="1524c-107">On .NET Framework and .NET Core, you can annotate methods with the <xref:System.Security.Permissions.PrincipalPermissionAttribute> attribute.</span></span> <span data-ttu-id="1524c-108">例如：</span><span class="sxs-lookup"><span data-stu-id="1524c-108">For example:</span></span>

```csharp
[PrincipalPermission(SecurityAction.Demand, Role = "Administrators")]
public void MyMethod()
{
    // Code that should only run when the current user is an administrator.
}
```

<span data-ttu-id="1524c-109">从 .NET 5 开始，不能将 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 属性应用于方法。</span><span class="sxs-lookup"><span data-stu-id="1524c-109">Starting in .NET 5, you cannot apply the <xref:System.Security.Permissions.PrincipalPermissionAttribute> attribute to a method.</span></span> <span data-ttu-id="1524c-110">该属性的构造函数已过时，会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1524c-110">The constructor for the attribute is obsolete and produces a compile-time error.</span></span> <span data-ttu-id="1524c-111">与其他过时警告不同，无法禁止显示此错误。</span><span class="sxs-lookup"><span data-stu-id="1524c-111">Unlike other obsoletion warnings, you can't suppress the error.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="1524c-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="1524c-112">Reason for change</span></span>

<span data-ttu-id="1524c-113"><xref:System.Security.Permissions.PrincipalPermissionAttribute> 类型与子类 <xref:System.Security.Permissions.SecurityAttribute> 等其他类型一样，是 .NET 的代码访问安全性 (CAS) 基础结构的一部分。</span><span class="sxs-lookup"><span data-stu-id="1524c-113">The <xref:System.Security.Permissions.PrincipalPermissionAttribute> type, like other types that subclass <xref:System.Security.Permissions.SecurityAttribute>, is part of .NET's Code Access Security (CAS) infrastructure.</span></span> <span data-ttu-id="1524c-114">在 .NET Framework 2.x - 4.x 中，即使应用程序在完全信任的情况下运行，运行时也会在方法条目上强制使用 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 批注。</span><span class="sxs-lookup"><span data-stu-id="1524c-114">In .NET Framework 2.x - 4.x, the runtime enforces <xref:System.Security.Permissions.PrincipalPermissionAttribute> annotations on method entry, even if the application is running under a full-trust scenario.</span></span> <span data-ttu-id="1524c-115">.NET Core 和 .NET 5 及更高版本不支持 CAS 属性，而且运行时将忽略这些属性。</span><span class="sxs-lookup"><span data-stu-id="1524c-115">.NET Core and .NET 5 and later don't support CAS attributes, and the runtime ignores them.</span></span>

<span data-ttu-id="1524c-116">从 .NET Framework 到 .NET Core 和 .NET 5 的行为差异可能导致“无法打开”的情况，在这种情况下，本应阻止但却允许了访问。</span><span class="sxs-lookup"><span data-stu-id="1524c-116">This difference in behavior from .NET Framework to .NET Core and .NET 5 can result in a "fail open" scenario, where access should have been blocked but instead has been allowed.</span></span> <span data-ttu-id="1524c-117">为防止出现“无法打开”的情况，你不能再在面向 .NET 5 或更高版本的代码中应用该属性。</span><span class="sxs-lookup"><span data-stu-id="1524c-117">To prevent the "fail open" scenario, you can no longer apply the attribute in code that targets .NET 5 or later.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="1524c-118">引入的版本</span><span class="sxs-lookup"><span data-stu-id="1524c-118">Version introduced</span></span>

<span data-ttu-id="1524c-119">5.0</span><span class="sxs-lookup"><span data-stu-id="1524c-119">5.0</span></span>

## <a name=""></a><span data-ttu-id="1524c-120"><a id="permission-action">建议的操作</a></span><span class="sxs-lookup"><span data-stu-id="1524c-120"><a id="permission-action">Recommended action</a></span></span>

<span data-ttu-id="1524c-121">如果遇到过时错误，则必须采取措施。</span><span class="sxs-lookup"><span data-stu-id="1524c-121">If you encounter the obsoletion error, you must take action.</span></span>

- <span data-ttu-id="1524c-122">**如果要将属性应用于 ASP.NET MVC 操作方法，请执行以下操作：**</span><span class="sxs-lookup"><span data-stu-id="1524c-122">**If you're applying the attribute to an ASP.NET MVC action method:**</span></span>

  <span data-ttu-id="1524c-123">考虑使用 ASP.NET 的内置授权基础结构。</span><span class="sxs-lookup"><span data-stu-id="1524c-123">Consider using ASP.NET's built-in authorization infrastructure.</span></span> <span data-ttu-id="1524c-124">以下代码演示如何使用 <xref:System.Web.Mvc.AuthorizeAttribute> 属性来为控制器添加批注。</span><span class="sxs-lookup"><span data-stu-id="1524c-124">The following code demonstrates how to annotate a controller with an <xref:System.Web.Mvc.AuthorizeAttribute> attribute.</span></span> <span data-ttu-id="1524c-125">ASP.NET 运行时将在执行操作之前向用户授权。</span><span class="sxs-lookup"><span data-stu-id="1524c-125">The ASP.NET runtime will authorize the user before performing the action.</span></span>

  ```csharp
  using Microsoft.AspNetCore.Authorization;

  namespace MySampleApp
  {
      [Authorize(Roles = "Administrator")]
      public class AdministrationController : Controller
      {
          public ActionResult MyAction()
          {
              // This code won't run unless the current user
              // is in the 'Administrator' role.
          }
      }
  }
  ```

  <span data-ttu-id="1524c-126">有关详细信息，请参阅 [ASP.NET Core 中基于角色的授权](/aspnet/core/security/authorization/roles)和 [ASP.NET Core 中的授权简介](/aspnet/core/security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="1524c-126">For more information, see [Role-based authorization in ASP.NET Core](/aspnet/core/security/authorization/roles) and [Introduction to authorization in ASP.NET Core](/aspnet/core/security/authorization/introduction).</span></span>

- <span data-ttu-id="1524c-127">**如果要将属性应用到 Web 应用上下文之外的库代码，请执行以下操作：**</span><span class="sxs-lookup"><span data-stu-id="1524c-127">**If you're applying the attribute to library code outside the context of a web app:**</span></span>

  <span data-ttu-id="1524c-128">在方法开始时手动执行检查。</span><span class="sxs-lookup"><span data-stu-id="1524c-128">Perform the checks manually at the beginning of your method.</span></span> <span data-ttu-id="1524c-129">这可以通过使用 <xref:System.Security.Principal.IPrincipal.IsInRole(System.String)?displayProperty=nameWithType> 方法来完成。</span><span class="sxs-lookup"><span data-stu-id="1524c-129">This can be done by using the <xref:System.Security.Principal.IPrincipal.IsInRole(System.String)?displayProperty=nameWithType> method.</span></span>

  ```csharp
  using System.Threading;

  void DoSomething()
  {
      if (Thread.CurrentPrincipal == null
          || !Thread.CurrentPrincipal.IsInRole("Administrators"))
      {
          throw new Exception("User is anonymous or isn't an admin.");
      }

      // Code that should run only when user is an administrator.
  }
  ```

## <a name="affected-apis"></a><span data-ttu-id="1524c-130">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="1524c-130">Affected APIs</span></span>

- <xref:System.Security.Permissions.PrincipalPermissionAttribute?displayProperty=fullName>

<!--

#### Category

- Core .NET libraries
- Security

### Affected APIs

- `T:System.Security.Permissions.PrincipalPermissionAttribute`

-->

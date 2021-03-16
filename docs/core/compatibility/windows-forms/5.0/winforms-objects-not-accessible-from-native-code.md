---
title: 中断性变更：WinForms 对象不可通过本机代码访问
description: 了解 .NET 5 中的中断性变更：不可再通过本机代码访问 Windows 窗体对象。
ms.date: 01/29/2021
ms.openlocfilehash: 823d37cb8115b8669b254878325a350809393e79
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256084"
---
# <a name="native-code-cant-access-windows-forms-objects"></a><span data-ttu-id="12a6b-103">本机代码无法访问 Windows 窗体对象</span><span class="sxs-lookup"><span data-stu-id="12a6b-103">Native code can't access Windows Forms objects</span></span>

<span data-ttu-id="12a6b-104">从 .NET 5 开发，你无法再通过本机代码访问 Windows 窗体对象了。</span><span class="sxs-lookup"><span data-stu-id="12a6b-104">Starting in .NET 5, you can no longer access Windows Forms objects from native code.</span></span>

## <a name="change-description"></a><span data-ttu-id="12a6b-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="12a6b-105">Change description</span></span>

<span data-ttu-id="12a6b-106">在旧版 .NET 中，某些 Windows 窗体类型被修饰为对 COM 互操作可见，因此可通过本机代码访问。</span><span class="sxs-lookup"><span data-stu-id="12a6b-106">In previous .NET versions, some Windows Forms types were decorated as visible to COM interop, and thus were accessible to native code.</span></span> <span data-ttu-id="12a6b-107">从 .NET 5 开始，Windows 窗体 API 对 COM 互操作不可见或不可通过本机代码访问。</span><span class="sxs-lookup"><span data-stu-id="12a6b-107">Starting in .NET 5, no Windows Forms API are visible to COM interop or accessible to native code.</span></span> <span data-ttu-id="12a6b-108">.NET 运行时不再直接支持创建自定义类型库。</span><span class="sxs-lookup"><span data-stu-id="12a6b-108">The .NET runtime no longer supports creating custom type libraries out of the box.</span></span> <span data-ttu-id="12a6b-109">此外，.NET 运行时也无法依赖于 .NET Framework 的类型库（这就要求保持类在 .NET Framework 中的形状）。</span><span class="sxs-lookup"><span data-stu-id="12a6b-109">In addition, the .NET runtime can't depend on the type library for .NET Framework (which would require maintaining the shape of classes as they were in .NET Framework).</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="12a6b-110">更改原因</span><span class="sxs-lookup"><span data-stu-id="12a6b-110">Reason for change</span></span>

- <span data-ttu-id="12a6b-111">从用于生成和查找类型库（TLB 文件）的枚举中删除 `ComVisible(true)`：由于 .NET Core 没有提供 WinForms TLB，因此保留这个特性没有价值。</span><span class="sxs-lookup"><span data-stu-id="12a6b-111">Removal of `ComVisible(true)` from enumerations that were used for type library (TLB file) generation and lookup: Since there is no WinForms TLB provided by .NET Core, there's no value in keeping this attribute.</span></span>
- <span data-ttu-id="12a6b-112">从 `AccessibleObject` 类中删除 `ComVisible(true)`：这些类是不可 CoCreateable 的（它们没有无参数构造函数），并且向 COM 公开已存在的实例不需要此特性。</span><span class="sxs-lookup"><span data-stu-id="12a6b-112">Removal of `ComVisible(true)` from `AccessibleObject` classes: The classes are not CoCreateable (they have no parameterless constructor), and exposing an already existing instance to COM does not require that attribute.</span></span>
- <span data-ttu-id="12a6b-113">从 `Control` 和 `Component` 类中删除 `ComVisible(true)`：这是用来允许通过 OLE/ActiveX 托管 WinForms 控件的（例如，在 VB6 或 MFC 中）。</span><span class="sxs-lookup"><span data-stu-id="12a6b-113">Removal of `ComVisible(true)` from `Control` and `Component` classes: This was used to allow hosting of WinForms controls via OLE/ActiveX, for example in VB6 or MFC.</span></span> <span data-ttu-id="12a6b-114">但是，这需要用于 WinForms 的 TLB （不再提供），以及基于注册表的激活（这也不能开箱即用）。</span><span class="sxs-lookup"><span data-stu-id="12a6b-114">However, this requires a TLB for WinForms, which is no longer provided, as well as registry-based activation, which also would not work out of the box.</span></span> <span data-ttu-id="12a6b-115">一般来说，没有维护基于 COM 的 WinForms 控件托管，因此支持被删除，而不是让它处于不受支持的状态。</span><span class="sxs-lookup"><span data-stu-id="12a6b-115">Generally, there was no maintenance of COM-based hosting of WinForms controls, so support was removed instead of leaving it in an unsupported state.</span></span>
- <span data-ttu-id="12a6b-116">从控件中删除 `ClassInterface` 特性：如果不支持通过 OLE/ActiveX 进行托管，则不再需要这些特性。</span><span class="sxs-lookup"><span data-stu-id="12a6b-116">Removal of `ClassInterface` attributes from controls: If hosting via OLE/ActiveX is not supported, these attributes aren't needed anymore.</span></span> <span data-ttu-id="12a6b-117">它们被保存在对象仍向 COM 公开且特性可能相关的其他位置。</span><span class="sxs-lookup"><span data-stu-id="12a6b-117">They are kept in other places where objects are still exposed to COM and the attribute may be relevant.</span></span>
- <span data-ttu-id="12a6b-118">从 `EventArgs` 中删除 `ComVisible(true)`：它们很可能与不再受支持的 OLE/ActiveX 托管一起使用。</span><span class="sxs-lookup"><span data-stu-id="12a6b-118">Removal of `ComVisible(true)` from `EventArgs`: They were most likely used with OLE/ActiveX hosting, which is no longer supported.</span></span> <span data-ttu-id="12a6b-119">它们也是不可 CoCreateable 的，所以此特性没有任何用途。</span><span class="sxs-lookup"><span data-stu-id="12a6b-119">They are not CoCreateable either, so the attribute has no purpose.</span></span> <span data-ttu-id="12a6b-120">此外，公开现有实例而不提供 TLB 也没有意义。</span><span class="sxs-lookup"><span data-stu-id="12a6b-120">Also, exposing existing instances without providing a TLB makes no sense.</span></span>
- <span data-ttu-id="12a6b-121">从委托中删除 `ComVisible(true)`：目的未知，但由于不再支持通过 ActiveX 托管 WinForms 控件，因此它不太可能有任何用处。</span><span class="sxs-lookup"><span data-stu-id="12a6b-121">Removal of `ComVisible(true)` from delegates: Purpose is unknown, but since ActiveX hosting of WinForms Controls is no longer supported, it's unlikely to have any usefulness.</span></span>
- <span data-ttu-id="12a6b-122">从某些非公共代码中删除 `ComVisible(true)`：唯一的潜在使用者是新的 Visual Studio 设计器，但如果没有指定 GUID，就不太可能仍然需要它。</span><span class="sxs-lookup"><span data-stu-id="12a6b-122">Removal of `ComVisible(true)` from some non-public code: The only potential consumer would be the new Visual Studio designer, but without a GUID specified, it's unlikely that it's still needed.</span></span>
- <span data-ttu-id="12a6b-123">从某些任意公共设计器类中删除 `ComVisible(true)`：旧版 Visual Studio 设计器可能已使用 COM 互操作来与这些类通信。</span><span class="sxs-lookup"><span data-stu-id="12a6b-123">Removal of `ComVisible(true)` from some arbitrary public designer classes: The old Visual Studio designer may have been using COM interop to talk to these classes.</span></span> <span data-ttu-id="12a6b-124">然而，旧版设计器不支持 .NET Core，所以很少有人会需要将这些设置为 `ComVisible`。</span><span class="sxs-lookup"><span data-stu-id="12a6b-124">However, the old designer doesn't support .NET Core, so few people would need these as `ComVisible`.</span></span>
- <span data-ttu-id="12a6b-125">`IWin32Window` 定义了与 .NET Framework 中定义的 GUID 相同的 GUID，这会带来危险的后果。</span><span class="sxs-lookup"><span data-stu-id="12a6b-125">`IWin32Window` defined the same GUID that was defined in .NET Framework, which has dangerous consequences.</span></span> <span data-ttu-id="12a6b-126">如果你需要与 .NET Framework 互操作，请使用 `ComImport`。</span><span class="sxs-lookup"><span data-stu-id="12a6b-126">If you require interop with .NET Framework, use `ComImport`.</span></span>
- <span data-ttu-id="12a6b-127">WinForms 管理的 `IDataObject` 被设置为 `ComVisible`。</span><span class="sxs-lookup"><span data-stu-id="12a6b-127">The WinForms managed `IDataObject` was made `ComVisible`.</span></span> <span data-ttu-id="12a6b-128">这不是必需的，对于 `IDataObject` COM 互操作，有一个单独的 `ComImport` 接口声明。</span><span class="sxs-lookup"><span data-stu-id="12a6b-128">This is not required, there is a separate `ComImport` interface declaration for `IDataObject` COM interop.</span></span> <span data-ttu-id="12a6b-129">将管理的 `IDataObject` 设置为 `ComVisible` 会适得其反，因为没有提供 TLB，封送处理总是会失败。</span><span class="sxs-lookup"><span data-stu-id="12a6b-129">It's counterproductive to have the managed `IDataObject` be `ComVisible`, since no TLB is provided and marshaling will always fail.</span></span> <span data-ttu-id="12a6b-130">另外，GUID 没有被指定，并且与 .NET Framework 不同，因此删除未记录的 IID 不太可能对客户产生负面影响。</span><span class="sxs-lookup"><span data-stu-id="12a6b-130">Also, the GUID was not specified and differed from .NET Framework, so its unlikely that removing an undocumented IID will affect customers negatively.</span></span>
- <span data-ttu-id="12a6b-131">删除 `ComVisible(false)`：当默认不向 COM 互操作公开类时，它们被放置在看似任意的位置上，并且是多余的。</span><span class="sxs-lookup"><span data-stu-id="12a6b-131">Removal of `ComVisible(false)`: Those are placed in seemingly arbitrary places and are redundant when the default is to not expose classes to COM interop.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="12a6b-132">引入的版本</span><span class="sxs-lookup"><span data-stu-id="12a6b-132">Version introduced</span></span>

<span data-ttu-id="12a6b-133">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="12a6b-133">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="12a6b-134">建议的操作</span><span class="sxs-lookup"><span data-stu-id="12a6b-134">Recommended action</span></span>

<span data-ttu-id="12a6b-135">下面的示例适用于 .NET Framework 和 .NET Core 3.1。</span><span class="sxs-lookup"><span data-stu-id="12a6b-135">The following example works on .NET Framework and .NET Core 3.1.</span></span> <span data-ttu-id="12a6b-136">此示例依赖于 .NET Framework 类型库，这个类型库允许 JavaScript 通过反射回叫窗体子类。</span><span class="sxs-lookup"><span data-stu-id="12a6b-136">This example relies on the .NET Framework type library, which allows the JavaScript to call back into the form subclass via reflection.</span></span>

```cs
[PermissionSet(SecurityAction.Demand, Name="FullTrust")]
[System.Runtime.InteropServices.ComVisibleAttribute(true)]
public class Form1 : Form
{
    private WebBrowser webBrowser1 = new WebBrowser();

    protected override void OnLoad(EventArgs e)
    {
        webBrowser1.AllowWebBrowserDrop = false;
        webBrowser1.IsWebBrowserContextMenuEnabled = false;
        webBrowser1.WebBrowserShortcutsEnabled = false;
        webBrowser1.ObjectForScripting = this;

        webBrowser1.DocumentText =
            "<html><body><button " +
            "onclick=\"window.external.Test('called from script code')\">" +
            "call client code from script code</button>" +
            "</body></html>";
    }

    public void Test(String message)
    {
        MessageBox.Show(message, "client code");
    }
}
```

<span data-ttu-id="12a6b-137">可采用两种方法使此示例在 .NET 5 及更高版本上运行：</span><span class="sxs-lookup"><span data-stu-id="12a6b-137">There are two possible ways to make the example work on .NET 5 and later versions:</span></span>

- <span data-ttu-id="12a6b-138">引入支持 `IDispatch` 的用户声明的 `ObjectForScripting` 对象（默认情况下应用，除非在项目级别显式更改）。</span><span class="sxs-lookup"><span data-stu-id="12a6b-138">Introduce a user-declared `ObjectForScripting` object that supports `IDispatch` (which is applied by default, unless changed explicitly at the project level).</span></span>

  ```cs
  public class MyScriptObject
  {
      private Form1 _form;

      public MyScriptObject(Form1 form)
      {
          _form = form;
      }

      public void Test(string message)
      {
          MessageBox.Show(message, "client code");
      }
  }

  public partial class Form1 : Form
  {
      protected override void OnLoad(EventArgs e)
      {
          ...

          // Works correctly.
          webBrowser1.ObjectForScripting = new MyScriptObject(this);

          ...
      }
  }
  ```

- <span data-ttu-id="12a6b-139">使用要公开的方法来声明接口。</span><span class="sxs-lookup"><span data-stu-id="12a6b-139">Declare an interface with the methods to expose.</span></span>

  ```cs
  public interface IForm1
  {
      void Test(string message);
  }

  [ComDefaultInterface(typeof(IForm1))]
  public partial class Form1 : Form, IForm1
  {
      protected override void OnLoad(EventArgs e)
      {
          ...

          // Works correctly.
          webBrowser1.ObjectForScripting = this;

          ...
      }
  }
  ```

## <a name="affected-apis"></a><span data-ttu-id="12a6b-140">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="12a6b-140">Affected APIs</span></span>

<span data-ttu-id="12a6b-141">所有 Windows 窗体 API。</span><span class="sxs-lookup"><span data-stu-id="12a6b-141">All Windows Forms APIs.</span></span>

<!--

### Category

- Windows Forms

-->

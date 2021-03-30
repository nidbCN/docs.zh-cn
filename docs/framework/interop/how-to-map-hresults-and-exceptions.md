---
title: 如何：映射 HRESULT 和异常
description: 查看如何将从 COM 方法返回的 HRESULT 值映射到 .NET 方法引发的异常。 运行时处理 COM 和 .NET 之间的转换。
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- interoperation with unmanaged code, HRESULTs
- exceptions, HRESULTs
- interoperation with unmanaged code, exceptions
- HRESULTs
- COM interop, HRESULTs
- COM interop, exceptions
ms.assetid: 610b364b-2761-429d-9c4a-afbc3e66f1b9
ms.openlocfilehash: 13798de1547428d54c10f83c41a5a402e2b3fb53
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872400"
---
# <a name="how-to-map-hresults-and-exceptions"></a>如何：映射 HRESULT 和异常

COM 方法通过返回 HRESULT 来报告错误；NET 方法通过引发异常来报告错误。 运行时处理这两者之间的转换。 NET Framework 中的每个异常类都将映射到 HRESULT。

 用户定义的异常类可指定任何适用的 HRESULT。 这些异常类可通过设置异常对象上的 `HResult` 字段来动态更改生成异常时要返回的 HRESULT。 异常的其他信息通过 `IErrorInfo` 接口提供给客户端，这是在非管理进程中的 NET 对象上实现的。

 如果要创建扩展 `System.Exception` 的类，需要在构造时设置 HRESULT 字段。 否则，基类将分配 HRESULT 值。 可以通过提供异常的构造函数的值，将新的异常类映射到现有的 HRESULT。

 请注意，如果线程上存在 `IErrorInfo`，运行时将忽略 `HRESULT`。  此行为可以发生在 `HRESULT` 和`IErrorInfo` 表示不同错误的事例中。

### <a name="to-create-a-new-exception-class-and-map-it-to-an-hresult"></a>创建新的异常类并将其映射到 HRESULT

1. 使用以下代码创建名为 `NoAccessException` 的新的异常类，并将其映射到 HRESULT `E_ACCESSDENIED`。

    ```cpp
    Class NoAccessException : public ApplicationException
    {
        NoAccessException () {
        HResult = E_ACCESSDENIED;
    }
    }
    CMyClass::MethodThatThrows
    {
    throw new NoAccessException();
    }
    ```

 可能会遇到同时使用托管和非托管代码的程序（使用任何编程语言）。 例如，以下代码示例中的自定义封送处理程序使用 `Marshal.ThrowExceptionForHR(int HResult)` 方法，以使用特定 HRESULT 值引发异常。 此方法查找 HRESULT，并生成适当的异常类型。 例如，以下代码片段中的 HRESULT 可生成 `ArgumentException`。

```cpp
CMyClass::MethodThatThrows
{
    Marshal.ThrowExceptionForHR(COR_E_ARGUMENT);
}
```

 下表提供从 HRESULT 到其在 NET 中的相似异常类的通用映射。 不带显式映射的 HRESULT 值映射到 `COMException`。 可以在 [dotnet/runtime 存储库](https://github.com/dotnet/runtime/blob/main/src/coreclr/vm/rexcep.h)中找到完整的最新映射。

|HRESULT|.NET 异常|
|-------------|--------------------|
|`COR_E_APPLICATION`|`ApplicationException`|
|`COR_E_ARGUMENT` 或 `E_INVALIDARG`|`ArgumentException`|
|`COR_E_ARGUMENTOUTOFRANGE`|`ArgumentOutOfRangeException`|
|`COR_E_ARITHMETIC or ERROR_ARITHMETIC_OVERFLOW`|`ArithmeticException`|
|`COR_E_ARRAYTYPEMISMATCH`|`ArrayTypeMismatchException`|
|`COR_E_BADIMAGEFORMAT or ERROR_BAD_FORMAT`|`BadImageFormatException`|
|`COR_E_DIRECTORYNOTFOUND or ERROR_PATH_NOT_FOUND`|`DirectoryNotFoundException`|
|`COR_E_DIVIDEBYZERO`|`DivideByZeroException`|
|`COR_E_DUPLICATEWAITOBJECT`|`DuplicateWaitObjectException`|
|`COR_E_ENDOFSTREAM`|`EndOfStreamException`|
|`COR_E_ENTRYPOINTNOTFOUND`|`EntryPointNotFoundException`|
|`COR_E_EXCEPTION`|`Exception`|
|`COR_E_EXECUTIONENGINE`|`ExecutionEngineException`|
|`COR_E_FIELDACCESS`|`FieldAccessException`|
|`COR_E_FILENOTFOUND or ERROR_FILE_NOT_FOUND`|`FileNotFoundException`|
|`COR_E_FORMAT`|`FormatException`|
|`COR_E_INDEXOUTOFRANGE`|`IndexOutOfRangeException`|
|`COR_E_INVALIDCAST or E_NOINTERFACE`|`InvalidCastException`|
|`COR_E_INVALIDFILTERCRITERIA`|`InvalidFilterCriteriaException`|
|`COR_E_INVALIDOPERATION`|`InvalidOperationException`|
|`COR_E_IO`|`IOException`|
|`COR_E_MEMBERACCESS`|`AccessException`|
|`COR_E_METHODACCESS`|`MethodAccessException`|
|`COR_E_MISSINGFIELD`|`MissingFieldException`|
|`COR_E_MISSINGMANIFESTRESOURCE`|`MissingManifestResourceException`|
|`COR_E_MISSINGMEMBER`|`MissingMemberException`|
|`COR_E_MISSINGMETHOD`|`MissingMethodException`|
|`COR_E_NOTFINITENUMBER`|`NotFiniteNumberException`|
|`E_NOTIMPL`|`NotImplementedException`|
|`COR_E_NOTSUPPORTED`|`NotSupportedException`|
|`COR_E_NULLREFERENCE orE_POINTER`|`NullReferenceException`|
|`COR_E_OUTOFMEMORY or`<br /><br /> `E_OUTOFMEMORY`|`OutOfMemoryException`|
|`COR_E_OVERFLOW`|`OverflowException`|
|`COR_E_PATHTOOLONG or ERROR_FILENAME_EXCED_RANGE`|`PathTooLongException`|
|`COR_E_RANK`|`RankException`|
|`COR_E_REFLECTIONTYPELOAD`|`ReflectionTypeLoadException`|
|`COR_E_SECURITY`|`SecurityException`|
|`COR_E_SERIALIZATION`|`SerializationException`|
|`COR_E_STACKOVERFLOW orERROR_STACK_OVERFLOW`|`StackOverflowException`|
|`COR_E_SYNCHRONIZATIONLOCK`|`SynchronizationLockException`|
|`COR_E_SYSTEM`|`SystemException`|
|`COR_E_TARGET`|`TargetException`|
|`COR_E_TARGETINVOCATION`|`TargetInvocationException`|
|`COR_E_TARGETPARAMCOUNT`|`TargetParameterCountException`|
|`COR_E_THREADINTERRUPTED`|`ThreadInterruptedException`|
|`COR_E_THREADSTATE`|`ThreadStateException`|
|`COR_E_TYPELOAD`|`TypeLoadException`|
|`COR_E_TYPEINITIALIZATION`|`TypeInitializationException`|
|`COR_E_VERIFICATION`|`VerificationException`|

 若要检索扩展的错误信息，托管客户端需要检查已生成的异常对象的字段。 若要异常对象提供有关错误的有用信息，COM 对象必须实现 `IErrorInfo` 接口。 运行时使用由 `IErrorInfo` 提供的信息来初始化异常对象。

 如果 COM 对象不支持 `IErrorInfo`，则运行时将使用默认值初始化异常对象。 下表列出与异常对象关联的每个字段，并标识了当 COM 对象支持 `IErrorInfo` 时的默认信息源。

 请注意，如果线程上存在 `IErrorInfo`，运行时将忽略 `HRESULT`。  此行为可以发生在 `HRESULT` 和`IErrorInfo` 表示不同错误的事例中。

|异常字段|来自 COM 的信息源|
|---------------------|------------------------------------|
|`ErrorCode`|从调用返回的 HRESULT。|
|`HelpLink`|如果 `IErrorInfo->HelpContext` 非零，则该字符串由 `IErrorInfo->GetHelpFile` 和“#”以及 `IErrorInfo->GetHelpContext` 连接而成。 否则，字符串将从 `IErrorInfo->GetHelpFile` 返回。|
|`InnerException`|始终为空引用（在 Visual Basic 中为 `Nothing`）。|
|`Message`|从 `IErrorInfo->GetDescription` 返回的字符串。|
|`Source`|从 `IErrorInfo->GetSource` 返回的字符串。|
|`StackTrace`|堆栈跟踪。|
|`TargetSite`|返回出现故障的 HRESULT 的方法的名称。|

 异常字段（例如 `Message`、`Source` 和 `StackTrace`）对 `StackOverflowException` 不可用。

## <a name="see-also"></a>请参阅

- [高级 COM 互操作性](/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))
- [异常](../../standard/exceptions/index.md)

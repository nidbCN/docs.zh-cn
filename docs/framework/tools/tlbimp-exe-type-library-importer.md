---
title: Tlbimp.exe（类型库导入程序）
description: 使用 Tlbimp.exe（类型库导入程序）。 此工具将在 COM 类型库中找到的类型定义转换为 CLR 程序集中的等效定义。
ms.date: 03/30/2017
helpviewer_keywords:
- type libraries [.NET Framework], importing
- importing type library
- Tlbimp.exe
- type definition conversion
- Type Library Importer
- type libraries
- converting type definitions
ms.assetid: ec0a8d63-11b3-4acd-b398-da1e37e97382
ms.openlocfilehash: b9b17d916373441f207363a9e8d7fe1aa2181dc3
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652940"
---
# <a name="tlbimpexe-type-library-importer"></a>Tlbimp.exe（类型库导入程序）

类型库导入程序将 COM 类型库中的类型定义转换为公共语言运行时程序集中的等效定义。 Tlbimp.exe 的输出是一个二进制文件（程序集），其中包含在原始类型库中定义的类型的运行时元数据。 可以使用 [Ildasm.exe](ildasm-exe-il-disassembler.md) 这样的工具检查此文件。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
tlbimp tlbFile [options]  
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|--------------|-----------------|  
|*tlbFile*|任何包含 COM 类型库的文件的名称。|  
  
|选项|描述|  
|------------|-----------------|  
|/asmversion: versionnumber|指定要生成的程序集的版本号。 以 *major.minor.build.revision* 格式指定 *versionnumber*。|  
|/company: `companyinformation`|将公司信息添加到输出程序集。|  
|/copyright: `copyrightinformation`|将版权信息添加到输出程序集。 此信息可在程序集的“文件属性”对话框中查看。|  
|**/delaysign**|指定 Tlbimp.exe 使用延迟签名对通过结果程序集进行强名称签名。 此选项必须与 **/keycontainer:** 、 **/keyfile:** 或 **/publickey:** 选项一起指定。 有关延迟签名过程的更多信息，请参见[延迟为程序集签名](../../standard/assembly/delay-sign.md)。|  
|**/help**|显示该工具的命令语法和选项。|  
|/keycontainer: containername|使用在 *containername* 指定的密钥容器中找到的公钥/私钥对，对结果程序集进行强名称签名。|  
|/keyfile: filename|使用在 *filename* 中找到的发行者的正式公钥/私钥对，对结果程序集进行强名称签名。|  
|/machine: `machinetype`|创建以指定计算机类型（微处理器）为目标的程序集。 支持的计算机类型：x86、x64、Itanium 和 Agnostic。|  
|/namespace: namespace|指定在其中生成程序集的命名空间。|  
|**/noclassmembers**|阻止 Tlbimp.exe 向类添加成员。 这样可避免潜在的 <xref:System.TypeLoadException>。|  
|**/nologo**|取消显示 Microsoft 启动版权标志。|  
|/out: filename|指定输出文件、程序集以及要写入元数据定义的命名空间的名称。 如果类型库指定的接口定义语言 (IDL) 自定义特性显式控制程序集的命名空间，则 **/out** 选项对程序集的命名空间没有任何影响。 如果你没有指定此选项，则 Tlbimp.exe 会将元数据写入到与在输入文件内定义的实际类型库同名的文件中，并且为其分配 .dll 扩展名。 如果输出文件的名称与输入文件的名称相同，则该工具将生成一个错误以防止覆盖类型库。|  
|**/primary**|生成指定类型库的主互操作程序集。 将在程序集中添加相关信息以指示类型库的发行者已生成程序集。 通过指定主互操作程序集，可以将发布者的程序集与使用 Tlbimp.exe 从类型库创建的任何其他程序集区分开来。 如果你是用 Tlbimp.exe 导入的类型库的发布者，则应只使用 **/primary** 选项。 请注意，你必须使用[强名称](../../standard/assembly/strong-named.md)对主互操作程序集进行签名。 有关详细信息，请参阅[主互操作程序集](/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100))。|  
|/product: `productinformation`|将产品信息添加到输出程序集。 此信息可在程序集的“文件属性”对话框中查看。|  
|/productversion: `productversioninformation`|将产品版本信息添加到输出程序集。 没有格式限制。 此信息可在程序集的“文件属性”对话框中查看。|  
|/publickey: filename|指定包含用来对结果程序集签名的公钥的文件。 如果指定 **/keyfile:** 或 **/keycontainer:** 选项而非 **/publickey:** ，则 Tlbimp.exe 将根据随 **/keyfile:** 或 **/keycontainer:** 一起提供的公钥/私钥对来生成公钥。 **/publickey:** 选项支持测试键和延迟签名方案。 文件的格式为 Sn.exe 生成的格式。 有关详细信息，请参阅 [强名称工具 (Sn.exe)](sn-exe-strong-name-tool.md) 中的 Sn.exe 的 **-p** 选项。|  
|/reference: filename|指定用来解析对在当前类型库外定义的类型的引用的程序集文件。 如果没有指定 **/reference** 选项，则 Tlbimp.exe 将自动以递归方式导入任何由导入的类型库引用的外部类型库。 如果指定了 **/reference** 选项，则在导入其他类型库之前，该工具将尝试解析被引用程序集中的外部类型。|  
|/silence: `warningnumber`|禁止显示指定的警告。 此选项不能与 **/silent** 一起使用。|  
|**/Silent**|取消显示成功消息。 此选项不能与 **/silence** 一起使用。|  
|**/strictref**|如果该工具不能解析当前程序集、 **/reference** 选项指定的程序集或已注册的主互操作程序集 (PIA) 内的所有引用，则不要导入类型库。|  
|**/strictref:nopia**|与 **/strictref** 相同，但忽略 PIA。|  
|**/sysarray**|指定该工具导入 COM 样式 SafeArray 作为托管 <xref:System.Array> 类型。|  
|/tlbreference: filename|指定用来在不参考注册表的情况下解析类型库引用的类型库文件。<br /><br /> 请注意，此选项将不加载某些较早的类型库格式。  但是，你仍可以通过注册表或当前目录隐式加载较早的类型库格式。|  
|/trademark: `trademarkinformation`|将商标信息添加到输出程序集。 此信息可在程序集的“文件属性”对话框中查看。|  
|/transform: transformname|按 *transformname* 参数指定的方式转换元数据。<br /><br /> 指定 **dispret** 作为 *transformname* 参数，可以将仅支持调度的接口的方法的 [out, retval] 参数转换为返回值。<br /><br /> 有关此选项的更多信息，请参见本主题后面的示例。|  
|**/unsafe**|在不进行 .NET Framework 安全性检查的情况下生成接口。 调用以此方式公开的方法可能会导致安全风险。 如果你不了解公开此类代码的风险，则不应使用此选项。|  
|**/verbose**|指定详细模式；显示有关导入的类型库的附加信息。|  
|**/VariantBoolFieldToBool**|将结构中的 `VARIANT_BOOL` 字段转换为 <xref:System.Boolean>。|  
|**/?**|显示该工具的命令语法和选项。|  
  
> [!NOTE]
> Tlbimp.exe 的命令行选项不区分大小写，并可以按任意顺序提供。 只需指定足够的选项来唯一标识它。 因此，/n 等效于 /nologo，而 /ou: outfile.dll 等效于 /out: outfile.dll。  
  
## <a name="remarks"></a>备注  

 Tlbimp.exe 同时执行整个类型库的转换。 不能使用该工具为在单个类型库中定义的类型的子集生成类型信息。  
  
 能够将[强名称](../../standard/assembly/strong-named.md)分配给程序集通常是有用或必需的。 因此，Tlbimp.exe 包括相应的选项，用以提供必需的信息来生成具有强名称的程序集。 **/keyfile:** 和 **/keycontainer:** 选项均使用强名称对程序集进行签名。 因此，一次只提供这些选项中的一个是合理的。  
  
 可通过多次使用 **/reference** 选项来指定多个引用程序集。

 由于 Tlbimp.exe 生成程序集的方式，因此无法将程序集重定向到其他 `mscorlib` 版本。 例如，如果希望生成面向 .NET Framework 2.0 的程序集，则必须使用 .NET Framework 2.0/3.0/3.5 SDK 附带的 Tlbimp.exe。 要面向 .NET Framework 4.x，应使用 .NET Framework 4.x SDK 附带的 Tlbimp.exe。

 在从包含多个类型库的模块中导入类型库时，可以选择将资源 ID 追加到一个类型库文件中。 仅当类型库文件位于当前目录中时或者当你指定了完整的路径时，Tlbimp.exe 才能找到该文件。 请参见本主题后面的示例。  
  
## <a name="examples"></a>示例  

 下面的命令生成一个与在 `myTest.tlb` 中找到的类型库具有相同的名称的程序集（具有 .dll 扩展名）。  
  
```console  
tlbimp myTest.tlb
```  
  
 下面的命令生成一个名为 `myTest.dll` 的程序集。  
  
```console  
tlbimp  myTest.tlb  /out:myTest.dll  
```  
  
 下面的命令生成一个与 `MyModule.dll\1` 指定的类型库具有相同的名称的程序集（具有 .dll 扩展名）。 `MyModule.dll\1` 必须位于当前目录中。  
  
```console  
tlbimp MyModule.dll\1  
```  
  
 下面的命令为类型库 `myTestLib.dll` 生成一个名为 `TestLib.dll` 的程序集。 **/transform:dispret** 选项将类型库中的调度接口方法的任何 [out, retval] 参数转换为托管库中的返回值。  
  
```console  
tlbimp TestLib.dll /transform:dispret /out:myTestLib.dll  
```  
  
 上例中的类型库 `TestLib.dll` 包含一个名为 `SomeMethod` 的调度接口方法，它返回 void 且具有一个 [out, retval] 参数。 下面的代码是 `SomeMethod` 中 `TestLib.dll` 的输入类型库方法签名。  
  
```cpp  
void SomeMethod([out, retval] VARIANT_BOOL*);  
```  
  
 指定 **/transform:dispret** 选项会使 Tlbimp.exe 将 `SomeMethod` 的 `[out, retval]` 参数转换为 `bool` 返回值。 下面是指定 **/transform:dispret** 选项时，Tlbimp.exe 为托管库 `myTestLib.dll` 中的 `SomeMethod` 生成的方法签名。  
  
```csharp  
bool SomeMethod();  
```  
  
 如果使用 Tlbimp.exe 生成 `TestLib.dll` 的托管库时没有指定 **/transform:dispret**，则该工具将为托管库 `myTestLib.dll` 中的 `SomeMethod` 生成以下方法签名。  
  
```csharp  
void SomeMethod(out bool x);  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Tlbexp.exe（类型库导出程序）](tlbexp-exe-type-library-exporter.md)
- [将类型库作为程序集导入](../interop/importing-a-type-library-as-an-assembly.md)
- [有关从类型库转换到程序集的摘要](/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Ildasm.exe（IL 反汇编程序）](ildasm-exe-il-disassembler.md)
- [Sn.exe（强名称工具）](sn-exe-strong-name-tool.md)
- [具有强名称的程序集](../../standard/assembly/strong-named.md)
- [用于向互操作程序集导入类型库的特性](/previous-versions/dotnet/netframework-4.0/y6a7ak23(v=vs.100))
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)

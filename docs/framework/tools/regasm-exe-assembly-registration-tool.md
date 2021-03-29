---
title: Regasm.exe（程序集注册工具）
description: 使用 Regasm.exe（程序集注册工具）。 读取程序集元数据，并将所需项添加到注册表中，允许 COM 客户端创建 .NET 类。
ms.date: 03/30/2017
helpviewer_keywords:
- Assembly Registration tool
- assemblies [.NET Framework], registering
- Regasm.exe
- registering assemblies
ms.assetid: e190e342-36ef-4651-a0b4-0e8c2c0281cb
ms.openlocfilehash: 81d835d280a3a55efc7644b2a081d033afe73349
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653317"
---
# <a name="regasmexe-assembly-registration-tool"></a>Regasm.exe（程序集注册工具）

程序集注册工具读取程序集中的元数据，并将所需项添加到注册表中。注册表允许 COM 客户端以透明方式创建 .NET Framework 类。 在注册一个类之后，任何 COM 客户端都可以像使用 COM 类一样使用它。 类仅在安装程序集时注册一次。 只有实际注册程序集中的类实例之后才能从 COM 中创建它们。

若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
regasm assemblyFile [options]
```

## <a name="parameters"></a>参数

|参数|说明|
|---------------|-----------------|
|assemblyFile|要向 COM 注册的程序集。|

|选项|描述|
|------------|-----------------|
|/codebase|在注册表中创建一个 Codebase 项。 Codebase 项指定未安装到全局程序集缓存中的程序集的文件路径。 如果随后将安装要注册到全局程序集缓存中的程序集，请勿指定此选项。 用 /codebase 选项指定的 assemblyFile 参数必须是[具有强名称的程序集](../../standard/assembly/strong-named.md)   。|
|/registered|指定此工具将仅引用已经注册的类型库。|
|/asmpath:directory|指定包含程序集引用的目录。 必须与 /regfile 选项一起使用  。|
|**/nologo**|取消显示 Microsoft 启动版权标志。|
|/regfile [: regFile]|为程序集生成指定的 .reg 文件，其中包含所需的注册表项。 指定此选项将不会更改注册表。 此选项不能与 /u 或 /tlb 选项一起使用   。|
|/silent 或 /s|取消显示成功消息。|
|/tlb [: typeLibFile]|从指定的程序集生成类型库，该类型库包含在程序集中定义的可访问类型的定义。|
|/unregister 或 /u|注销在 assemblyFile 中找到的可创建类  。 省略此选项将使 Regasm.exe 注册程序集中的可创建类。|
|**/verbose**|指定详细模式；当与 /tlb 选项一起指定时，将显示所有需要为其生成类型库的引用程序集的列表  。|
|**/?** 或 /help|显示该工具的命令语法和选项。|

> [!NOTE]
> Regasm.exe 命令行选不区分大小写。 只需提供足以唯一地进行标识的选项部分。 例如，/n 等效于 /nologo，而 /t: outfile.tlb 等效于 /tlb: outfile.tlb       。

## <a name="remarks"></a>备注

可以使用 /regfile 选项生成包含注册表项的 .reg 文件，而不是直接对注册表进行更改  。 通过注册表编辑器工具 (Regedit.exe) 导入 .reg 文件，可以在计算机上更新注册表。 .reg 文件不包含任何可由用户定义的注册函数完成的注册表更新。 /regfile 选项只为托管类发出注册表项  。 此选项不为 `TypeLibID` 或 `InterfaceID` 发出注册表项。

指定 /tlb 选项时，Regasm.exe 将生成并注册一个类型库，对在程序集中找到的类型进行描述  。 Regasm.exe 将生成的类型库放置在当前的工作目录中或为输出文件指定的目录中。 为引用其他程序集的程序集生成类型库可能导致同时生成几个类型库。 你可使用类型库向开发工具（如 Visual Studio）提供类型信息。 如果所注册的程序集是由类型库导入程序 ([Tlbimp.exe](tlbimp-exe-type-library-importer.md)) 生成的，请勿使用 /tlb 选项  。 如果程序集是从类型库导入的，则不能从它导出类型库。 除了类型库导出程序 ([Tlbexp.exe](tlbexp-exe-type-library-exporter.md)) 不会注册其生成的类型库外，使用 /tlb 选项与使用 Tlbexp.exe 和 Regasm.exe 的效果相同  。  如果使用 /tlb  选项注册某个类型库，则可将 /tlb  选项和 /unregister  选项一起使用以注销类型库。 将两个选项一起使用将注销类型库和接口项，这样可较大程度地清理注册表。

当你注册一个程序集供 COM 使用时，Regasm.exe 会在本地计算机上的注册表中添加一些项。 具体而言，它将创建一些与版本相关的注册表键，从而允许在一台计算机上并行运行同一程序集的多个版本。 在第一次注册某个程序集时，将会为该程序集创建一个顶级键，并为特定版本创建一个唯一的子键。 每次注册该程序集的新版本时，Regasm.exe 都将为新版本创建一个子键。

例如，假设你注册了一个版本为 1.0.0.0 的托管组件 myComp.dll 供 COM 使用。 之后，你注册了版本为 2.0.0.0 的 myComp.dll。 你确定计算机上的所有 COM 客户端应用程序都要使用 2.0.0.0 版本的 myComp.dll，并且决定注销 myComponent.dll 版本 1.0.0.0。 此注册表方案允许你注销 myComp.dll 版本 1.0.0.0，这是因为注销操作只移除 1.0.0.0 版本子键。

使用 Regasm.exe 注册一个程序集之后，可以将该程序集安装在[全局程序集缓存](../app-domains/gac.md)中，以便可以从任何 COM 客户端激活它。 如果该程序集将仅由单个应用程序激活，则可以将其放置在该应用程序的目录中。

## <a name="examples"></a>示例

下面的命令将注册 `myTest.dll` 中包含的所有公共类。

```console
regasm myTest.dll
```

下面的命令生成包含所有必要的注册表项的 `myTest.reg` 文件。 此命令不更新注册表。

```console
regasm myTest.dll /regfile:myTest.reg
```

下面的命令注册 `myTest.dll` 中包含的所有公共类，并生成和注册类型库 `myTest.tlb`，该类型库包含 `myTest.dll` 中定义的所有公共类型的定义。

```console
regasm myTest.dll /tlb:myTest.tlb
```

## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Tlbexp.exe（类型库导出程序）](tlbexp-exe-type-library-exporter.md)
- [Tlbimp.exe（类型库导入程序）](tlbimp-exe-type-library-importer.md)
- [向 COM 注册程序集](../interop/registering-assemblies-with-com.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)

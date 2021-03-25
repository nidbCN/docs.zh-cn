---
description: 用于安全性的 C# 编译器选项。 三个选项将控制签名程序集或地址空间布局。
title: C# 编译器选项 - 安全选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- PublicSign compiler option [C#]
- DelaySign compiler option [C#]
- KeyFile compiler option [C#]
- KeyContainer compiler option [C#]
- HighEntropyVA compiler option [C#]
ms.openlocfilehash: db7b612fa2e4ddb566ac2ba5ef9e4e66c5f0b0ab
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482406"
---
# <a name="c-compiler-options-for-security-options"></a>用于安全选项的 C# 编译器选项

以下选项可控制编译器安全选项。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **PublicSign** / `-publicsign`：公开对程序集签名。
- **DelaySign** / `-delaysign`：仅使用强名称密钥的公共部分对程序集进行延迟签名。
- **KeyFile** / `-keyfile`：指定强名称密钥文件。
- **KeyContainer** / `-keycontainer`：指定强名称密钥容器。
- **HighEntropyVA** / `-highentropyva`：启用高熵地址空间布局随机化 (ASLR)

## <a name="publicsign"></a>PublicSign

此选项会导致编译器应用公钥，但不会实际对程序集签名。 PublicSign 选项还会在程序集中设置位，以告知运行时该文件已签名。

```xml
<PublicSign>true</PublicSign>
```

PublicSign 选项需要使用 [KeyFile](#keyfile) 或 [KeyContainer](#keycontainer) 选项。 keyFile 和 KeyContainer 选项指定公钥。 PublicSign 和 PublicSign 选项互斥。 公共签名有时称为“假签名”或“OSS 签名”，它包括输出程序集中的公钥并设置“已签名”标记。 公共签名实际上并不使用私钥对程序集进行签名。 开发人员为开放源代码项目使用公共签名。  当人们无权访问用于对程序集进行签名的私钥时，他们会生成与已发布的“完全签名”程序集兼容的程序集。 由于很少有使用者实际需要检查程序集是否完全签名，因此这些公开生成的程序集几乎适用于每个使用完全签名程序集的方案。

## <a name="delaysign"></a>DelaySign

此选项会使编译器在输出文件中保留空间，以便以后添加数字签名。

```xml
<DelaySign>true</DelaySign>
```

如果需要完全签名的程序集，请使用 DelaySign-。 如果仅需要将公钥置于程序集中，则使用 DelaySign。 除非与 [KeyFile](#keyfile) 或 [KeyContainer](#keycontainer) 一同使用，否则 DelaySign 选项将不起作用。 [KeyContainer](#keycontainer) 和 [PublicSign](#publicsign) 选项互斥。 在请求完全签名的程序集时，编译器会对包含清单（程序集元数据）的文件进行哈希处理，并使用私钥对哈希进行签名。 该操作创建一个数字签名，它存储在包含清单的文件中。 在对程序集延迟签名时，编译器不会计算和存储签名。 相反，编译器会在文件中保留空间，便于稍后添加签名。

使用 DelaySign，测试人员可以将程序集放入全局缓存中。 测试完成后，可使用[程序集链接器](../../../framework/tools/al-exe-assembly-linker.md)实用工具将私钥置于程序集中，对程序集进行完全签名。 有关详细信息，请参阅[创建和使用具有强名称的程序集](../../../standard/assembly/create-use-strong-named.md)和[延迟为程序集签名](../../../standard/assembly/delay-sign.md)。

## <a name="keyfile"></a>KeyFile

指定包含加密密钥的文件名。

```xml
<KeyFile>filename</KeyFile>
```

`file` 是包含强名称密钥的文件的名称。 使用此选项时，编译器在程序集清单中插入指定字段的公钥，然后使用私钥对最终的程序集进行签名。 若要生成密钥文件，请在命令行键入 `sn -k file`。 如果你使用 [-target:module](output.md#targettype) 进行编译，密钥文件的名称将保存在模块中，并在你使用 [AddModules](inputs.md#addmodules) 编译程序集时，合并到创建的程序集中。 你也可以使用 [Keycontainer](#keycontainer) 将加密信息传递给编译器。 如果需要部分签名的程序集，请使用 [DelaySign](#delaysign)。 如果在同一编译中同时指定 KeyFile 和 KeyContainer，则编译器将首先尝试使用密钥容器。 如果成功，则使用密钥容器中的信息对程序集签名。 如果编译器没有找到密钥容器，它将尝试使用通过 [KeyFile](#keyfile) 指定的文件。 如果成功，则使用密钥文件中的信息对程序集签名，并且将密钥信息安装到密钥容器中。 在下一次编译中，密钥容器将生效。 密钥文件可能仅包含公钥。 有关详细信息，请参阅[创建和使用具有强名称的程序集](../../../standard/assembly/create-use-strong-named.md)和[延迟为程序集签名](../../../standard/assembly/delay-sign.md)。

## <a name="keycontainer"></a>KeyContainer

指定加密密钥容器的名称。

```xml
<KeyContainer>container</KeyContainer>
```

`container` 是强名称密钥容器的名称。 当使用 KeyContainer 选项时，编译器将创建一个可共享的组件。 编译器在程序集清单中插入指定容器的公钥，然后使用私钥对最终的程序集进行签名。 若要生成密钥文件，请在命令行键入 `sn -k file`。 `sn -i` 将密钥对安装到容器中。 编译器在 CoreCLR 上运行时，不支持此选项。 若要在基于 CoreCLR 生成时对程序集进行签名，请使用 [KeyFile](#keyfile) 选项。 如果你使用 [TargetType](output.md#targettype) 进行编译，那么，当你使用 [AddModules](inputs.md#addmodules) 将此模块编译到程序集时，密钥文件的名称将保存在模块中，而且会并入程序集。 还可以将此选项指定为任何 Microsoft 中间语言 (MSIL) 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyKeyNameAttribute?displayProperty=nameWithType>)。 此外，可使用 [KeyFile](#keyfile) 将加密信息传递给编译器。 使用 [DelaySign](#delaysign) 可将公钥添加到程序集清单中，但在程序集被测试之前，会对其进行签名。 有关详细信息，请参阅[创建和使用具有强名称的程序集](../../../standard/assembly/create-use-strong-named.md)和[延迟为程序集签名](../../../standard/assembly/delay-sign.md)。

## <a name="highentropyva"></a>HighEntropyVA

HighEntropyVA 编译器选项可告知 Windows 内核，特定的可执行文件是否支持高熵地址空间布局随机化 (ASLR)。

```xml
<HighEntropyVA>true</HighEntropyVA>
```

此选项指定 64 位可执行文件或由 [PlatformTarget](output.md#platformtarget) 编译器选项标记的可执行文件支持高熵虚拟地址空间。 默认情况下，此选项处于禁用状态。 可以使用 HighEntropyVA 启用它。

当随机化进程的地址空间布局包含在 ASLR 中时，HighEntropyVA 选项允许 Windows 内核的兼容版本使用更高程度的熵。 使用更高程度的熵意味着，可向内存区域（例如堆栈或堆）分配更多的地址。 因此，猜测特定内存区域的位置会更加困难。 HighEntropyVA 编译器选项需要目标可执行文件及其依赖的任何模块在作为 64 位进程运行时，能够处理大于 4 GB 的指针值。

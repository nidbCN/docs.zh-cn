---
description: 了解详细信息： BC30145：无法发出程序集： <error message>
title: 无法发出程序集：<error message>
ms.date: 08/14/2018
f1_keywords:
- vbc30145
- bc30145
helpviewer_keywords:
- BC30145
ms.assetid: 2e7eb2b9-eda6-4bdb-95cc-72c7f0be7528
ms.openlocfilehash: 2ba476b39b6aa441d8778ee0618dcc3dcf3f7ac8
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653272"
---
# <a name="bc30145-unable-to-emit-assembly-error-message"></a><span data-ttu-id="d393c-103">BC30145：无法发出程序集： \<error message></span><span class="sxs-lookup"><span data-stu-id="d393c-103">BC30145: Unable to emit assembly: \<error message></span></span>

<span data-ttu-id="d393c-104">Visual Basic 编译器调用程序集链接器 (*Al.exe*（也称为 Alink) ）以生成包含清单的程序集，并且链接器在创建该程序集的辐射阶段报告错误。</span><span class="sxs-lookup"><span data-stu-id="d393c-104">The Visual Basic compiler calls the Assembly Linker (*Al.exe*, also known as Alink) to generate an assembly with a manifest, and the linker reports an error in the emission stage of creating the assembly.</span></span>

<span data-ttu-id="d393c-105">**错误 ID：** BC30145</span><span class="sxs-lookup"><span data-stu-id="d393c-105">**Error ID:** BC30145</span></span>

## <a name="to-correct-this-error"></a><span data-ttu-id="d393c-106">更正此错误</span><span class="sxs-lookup"><span data-stu-id="d393c-106">To correct this error</span></span>

1. <span data-ttu-id="d393c-107">检查引用的错误消息并参考主题 [Al.exe](../../../framework/tools/al-exe-assembly-linker.md) 以获取进一步的说明和建议。</span><span class="sxs-lookup"><span data-stu-id="d393c-107">Examine the quoted error message and consult the topic [Al.exe](../../../framework/tools/al-exe-assembly-linker.md) for further explanation and advice.</span></span>

2. <span data-ttu-id="d393c-108">尝试使用 [Al.exe](../../../framework/tools/al-exe-assembly-linker.md) 或 [Sn.exe (强名称工具) ](../../../framework/tools/sn-exe-strong-name-tool.md)，手动对程序集进行签名。</span><span class="sxs-lookup"><span data-stu-id="d393c-108">Try signing the assembly manually, using either the [Al.exe](../../../framework/tools/al-exe-assembly-linker.md) or the [Sn.exe (Strong Name Tool)](../../../framework/tools/sn-exe-strong-name-tool.md).</span></span>

3. <span data-ttu-id="d393c-109">如果仍然出现错误，则收集有关该情况的信息并通知 Microsoft 产品支持服务。</span><span class="sxs-lookup"><span data-stu-id="d393c-109">If the error persists, gather information about the circumstances and notify Microsoft Product Support Services.</span></span>

### <a name="to-sign-the-assembly-manually"></a><span data-ttu-id="d393c-110">手动对程序集进行签名</span><span class="sxs-lookup"><span data-stu-id="d393c-110">To sign the assembly manually</span></span>

1. <span data-ttu-id="d393c-111">使用 [Sn.exe (强名称工具) ](../../../framework/tools/sn-exe-strong-name-tool.md)) 来创建公钥/私钥对文件。</span><span class="sxs-lookup"><span data-stu-id="d393c-111">Use the [Sn.exe (Strong Name Tool)](../../../framework/tools/sn-exe-strong-name-tool.md)) to create a public/private key pair file.</span></span>

   <span data-ttu-id="d393c-112">此文件的扩展名为 *.snk* 。</span><span class="sxs-lookup"><span data-stu-id="d393c-112">This file has an *.snk* extension.</span></span>

2. <span data-ttu-id="d393c-113">从项目中删除生成错误的 COM 引用。</span><span class="sxs-lookup"><span data-stu-id="d393c-113">Delete the COM reference that is generating the error from your project.</span></span>

3. <span data-ttu-id="d393c-114">打开 [Visual studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="d393c-114">Open [Visual Studio Developer Command Prompt or Visual Studio Developer PowerShell](/visualstudio/ide/reference/command-prompt-powershell).</span></span>

4. <span data-ttu-id="d393c-115">将目录更改为要在其中放置程序集包装的目录。</span><span class="sxs-lookup"><span data-stu-id="d393c-115">Change the directory to the directory where you want to place your assembly wrapper.</span></span>

5. <span data-ttu-id="d393c-116">输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="d393c-116">Enter the following command:</span></span>

    ```cmd
    tlbimp <path to COM reference file> /out:<output assembly name> /keyfile:<path to .snk file>
    ```

   <span data-ttu-id="d393c-117">你可能会输入的实际命令的示例如下：</span><span class="sxs-lookup"><span data-stu-id="d393c-117">An example of the actual command you might enter is:</span></span>

    ```cmd
    tlbimp c:\windows\system32\msi.dll /out:Interop.WindowsInstaller.dll /keyfile:"c:\documents and settings\mykey.snk"
    ```

   > [!TIP]
   > <span data-ttu-id="d393c-118">如果路径或文件包含空格，请使用双引号。</span><span class="sxs-lookup"><span data-stu-id="d393c-118">Use double quotation marks if a path or file contains spaces.</span></span>

6. <span data-ttu-id="d393c-119">在 Visual Studio 中，将 .NET 程序集引用添加到刚创建的文件。</span><span class="sxs-lookup"><span data-stu-id="d393c-119">In Visual Studio, add a .NET Assembly reference to the file you just created.</span></span>

## <a name="see-also"></a><span data-ttu-id="d393c-120">请参阅</span><span class="sxs-lookup"><span data-stu-id="d393c-120">See also</span></span>

- [<span data-ttu-id="d393c-121">Al.exe</span><span class="sxs-lookup"><span data-stu-id="d393c-121">Al.exe</span></span>](../../../framework/tools/al-exe-assembly-linker.md)
- [<span data-ttu-id="d393c-122">Sn.exe（强名称工具）</span><span class="sxs-lookup"><span data-stu-id="d393c-122">Sn.exe (Strong Name Tool)</span></span>](../../../framework/tools/sn-exe-strong-name-tool.md)
- [<span data-ttu-id="d393c-123">如何：创建公钥/私钥对</span><span class="sxs-lookup"><span data-stu-id="d393c-123">How to: Create a Public-Private Key Pair</span></span>](../../../standard/assembly/create-public-private-key-pair.md)
- [<span data-ttu-id="d393c-124">Visual Studio 反馈选项</span><span class="sxs-lookup"><span data-stu-id="d393c-124">Visual Studio feedback options</span></span>](/visualstudio/ide/feedback-options)

---
title: CorFlags.exe（CorFlags 转换工具）
description: 了解 CorFlags.exe（CorFlags 转换工具）。 此工具可用于配置可移植可执行映像的标头的 CorFlags 部分。
ms.date: 03/30/2017
helpviewer_keywords:
- CorFlags conversion tool
- CorFlags.exe
- portable executable files, CorFlags section
ms.assetid: ef900f8f-71ca-4dde-9b8c-95ddb0d7d89c
ms.openlocfilehash: 2b801ba28e0513c899123daa98fd649cbc3d9c28
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654162"
---
# <a name="corflagsexe-corflags-conversion-tool"></a><span data-ttu-id="8e36a-104">CorFlags.exe（CorFlags 转换工具）</span><span class="sxs-lookup"><span data-stu-id="8e36a-104">CorFlags.exe (CorFlags Conversion Tool)</span></span>

<span data-ttu-id="8e36a-105">利用 CorFlags 转换工具，你配置可移植可执行映像标头的 CorFlags 部分。</span><span class="sxs-lookup"><span data-stu-id="8e36a-105">The CorFlags Conversion tool allows you to configure the CorFlags section of the header of a portable executable image.</span></span>  
  
 <span data-ttu-id="8e36a-106">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="8e36a-106">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="8e36a-107">若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="8e36a-107">To run the tool, use [Visual Studio Developer Command Prompt or Visual Studio Developer PowerShell](/visualstudio/ide/reference/command-prompt-powershell).</span></span>  
  
 <span data-ttu-id="8e36a-108">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="8e36a-108">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="8e36a-109">语法</span><span class="sxs-lookup"><span data-stu-id="8e36a-109">Syntax</span></span>  
  
```console  
CorFlags.exe assembly [options]  
```  
  
## <a name="parameters"></a><span data-ttu-id="8e36a-110">参数</span><span class="sxs-lookup"><span data-stu-id="8e36a-110">Parameters</span></span>  
  
|<span data-ttu-id="8e36a-111">必选参数</span><span class="sxs-lookup"><span data-stu-id="8e36a-111">Required parameter</span></span>|<span data-ttu-id="8e36a-112">描述</span><span class="sxs-lookup"><span data-stu-id="8e36a-112">Description</span></span>|  
|------------------------|-----------------|  
|`assembly`|<span data-ttu-id="8e36a-113">要为其配置 CorFlags 的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="8e36a-113">The name of the assembly for which to configure the CorFlags.</span></span>|  
  
|<span data-ttu-id="8e36a-114">选项</span><span class="sxs-lookup"><span data-stu-id="8e36a-114">Option</span></span>|<span data-ttu-id="8e36a-115">说明</span><span class="sxs-lookup"><span data-stu-id="8e36a-115">Description</span></span>|  
|:------------|-----------------|  
|`-32BIT[REQ]+`|<span data-ttu-id="8e36a-116">设置 32BITREQUIRED 标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-116">Sets the 32BITREQUIRED flag.</span></span>|  
|`-32BIT[REQ]-`|<span data-ttu-id="8e36a-117">清除 32BITREQUIRED 标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-117">Clears the 32BITREQUIRED flag.</span></span>|  
|`-32BITPREF+`|<span data-ttu-id="8e36a-118">设置 32BITPREFERRED 标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-118">Sets the 32BITPREFERRED flag.</span></span> <span data-ttu-id="8e36a-119">该应用程序甚至可以作为 32 位进程在 64 位平台上运行。</span><span class="sxs-lookup"><span data-stu-id="8e36a-119">The app runs as a 32-bit process even on 64-bit platforms.</span></span> <span data-ttu-id="8e36a-120">仅在 EXE 文件中设置此标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-120">Set this flag only on EXE files.</span></span> <span data-ttu-id="8e36a-121">如果在 DLL 上设置此标志，则 DLL 将无法在 64 位进程中加载，并引发 <xref:System.BadImageFormatException> 异常。</span><span class="sxs-lookup"><span data-stu-id="8e36a-121">If the flag is set on a DLL, the DLL fails to load in 64-bit processes, and a <xref:System.BadImageFormatException> exception is thrown.</span></span> <span data-ttu-id="8e36a-122">具有此标志的 EXE 文件可以加载到 64 位进程中。</span><span class="sxs-lookup"><span data-stu-id="8e36a-122">An EXE file with this flag can be loaded into a 64-bit process.</span></span><br /><br /> <span data-ttu-id="8e36a-123">.NET Framework 4.5 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="8e36a-123">New in the .NET Framework 4.5.</span></span>|  
|`-32BITPREF-`|<span data-ttu-id="8e36a-124">清除 32BITPREFERRED 标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-124">Clears the 32BITPREFERRED flag.</span></span><br /><br /> <span data-ttu-id="8e36a-125">.NET Framework 4.5 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="8e36a-125">New in the .NET Framework 4.5.</span></span>|  
|`-?`|<span data-ttu-id="8e36a-126">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="8e36a-126">Displays command syntax and options for the tool.</span></span>|  
|`-Force`|<span data-ttu-id="8e36a-127">强制执行更新，即使程序集具有强名称也是如此。</span><span class="sxs-lookup"><span data-stu-id="8e36a-127">Forces an update even if the assembly is strong-named.</span></span> <span data-ttu-id="8e36a-128">**重要提示：** 如果更新具有强名称的程序集，则必须在执行其代码之前再次对其签名。</span><span class="sxs-lookup"><span data-stu-id="8e36a-128">**Important:**  If you update a strong-named assembly, you must sign it again before executing its code.</span></span>|  
|`-help`|<span data-ttu-id="8e36a-129">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="8e36a-129">Displays command syntax and options for the tool.</span></span>|  
|`-ILONLY+`|<span data-ttu-id="8e36a-130">设置 ILONLY 标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-130">Sets the ILONLY flag.</span></span>|  
|`-ILONLY-`|<span data-ttu-id="8e36a-131">清除 ILONLY 标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-131">Clears the ILONLY flag.</span></span>|  
|`-nologo`|<span data-ttu-id="8e36a-132">取消显示 Microsoft 启动版权标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-132">Suppresses the Microsoft startup banner display.</span></span>|  
|`-RevertCLRHeader`|<span data-ttu-id="8e36a-133">将 CLR 标头版本还原到 2.0。</span><span class="sxs-lookup"><span data-stu-id="8e36a-133">Reverts the CLR header version to 2.0.</span></span>|  
|`-UpgradeCLRHeader`|<span data-ttu-id="8e36a-134">将 CLR 标头版本升级到 2.5。</span><span class="sxs-lookup"><span data-stu-id="8e36a-134">Upgrades the CLR header version to 2.5.</span></span> <span data-ttu-id="8e36a-135">**注意：** 程序集必须具有 2.5 版或更高版本的 CLR 标头才能在本机运行。</span><span class="sxs-lookup"><span data-stu-id="8e36a-135">**Note:**  Assemblies must have a CLR header version of 2.5 or greater to run natively.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="8e36a-136">备注</span><span class="sxs-lookup"><span data-stu-id="8e36a-136">Remarks</span></span>  

 <span data-ttu-id="8e36a-137">如果未指定任何选项，则 CorFlags 转换工具将显示指定程序集的标志。</span><span class="sxs-lookup"><span data-stu-id="8e36a-137">If no options are specified, the CorFlags Conversion tool displays the flags for the specified assembly.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="8e36a-138">请参阅</span><span class="sxs-lookup"><span data-stu-id="8e36a-138">See also</span></span>

- [<span data-ttu-id="8e36a-139">工具</span><span class="sxs-lookup"><span data-stu-id="8e36a-139">Tools</span></span>](index.md)
- [<span data-ttu-id="8e36a-140">64 位应用程序</span><span class="sxs-lookup"><span data-stu-id="8e36a-140">64-bit Applications</span></span>](../64-bit-apps.md)
- [<span data-ttu-id="8e36a-141">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="8e36a-141">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)

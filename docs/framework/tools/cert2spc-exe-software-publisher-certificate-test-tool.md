---
title: Cert2spc.exe（软件发行者证书测试工具）
description: 使用 Cert2spc.exe（软件发行者证书测试工具）。 此工具通过一个或多个 X.509 证书创建发行者证书 (SPC)。
ms.date: 03/30/2017
helpviewer_keywords:
- SPC
- Software Publisher Certificate Test tool
- Software Publisher Certificate
- Cert2spc.exe
- certificates, Software Publisher's Certificate
ms.assetid: be434d7d-9c0d-46e7-8392-58a9b542d11d
ms.openlocfilehash: 96b4c05f6c9af6fc78aa55b248a88de84e2d4ac8
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102258270"
---
# <a name="cert2spcexe-software-publisher-certificate-test-tool"></a><span data-ttu-id="6ddc9-104">Cert2spc.exe（软件发行者证书测试工具）</span><span class="sxs-lookup"><span data-stu-id="6ddc9-104">Cert2spc.exe (Software Publisher Certificate Test Tool)</span></span>

<span data-ttu-id="6ddc9-105">软件发行者证书测试工具从一个或多个 X.509 证书创建软件发行者证书 (SPC)。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-105">The Software Publisher Certificate Test tool creates a Software Publisher's Certificate (SPC) from one or more X.509 certificates.</span></span> <span data-ttu-id="6ddc9-106">Cert2spc.exe 仅用于测试目的。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-106">Cert2spc.exe is for test purposes only.</span></span> <span data-ttu-id="6ddc9-107">可以从证书颁发机构（如 VeriSign 或 Thawte）获得有效 SPC。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-107">You can obtain a valid SPC from a Certification Authority such as VeriSign or Thawte.</span></span> <span data-ttu-id="6ddc9-108">有关创建 X.509 证书的详细信息，请参阅 [Makecert.exe（证书创建工具）](/windows/desktop/SecCrypto/makecert)。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-108">For more information about creating X.509 certificates, see [Makecert.exe (Certificate Creation Tool)](/windows/desktop/SecCrypto/makecert).</span></span>  
  
 <span data-ttu-id="6ddc9-109">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-109">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="6ddc9-110">若要运行此工具，请使用[开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-110">To run the tool, use a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell).</span></span>  
  
 <span data-ttu-id="6ddc9-111">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="6ddc9-111">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="6ddc9-112">语法</span><span class="sxs-lookup"><span data-stu-id="6ddc9-112">Syntax</span></span>  
  
```console  
cert2spc cert1.cer | crl1.crl [... certN.cer | crlN.crl] outputSPCfile.spc  
```  
  
## <a name="parameters"></a><span data-ttu-id="6ddc9-113">参数</span><span class="sxs-lookup"><span data-stu-id="6ddc9-113">Parameters</span></span>  
  
|<span data-ttu-id="6ddc9-114">参数</span><span class="sxs-lookup"><span data-stu-id="6ddc9-114">Argument</span></span>|<span data-ttu-id="6ddc9-115">说明</span><span class="sxs-lookup"><span data-stu-id="6ddc9-115">Description</span></span>|  
|--------------|-----------------|  
|`certN.cer`|<span data-ttu-id="6ddc9-116">要包含在 SPC 文件中的 X.509 证书的名称。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-116">The name of an X.509 certificate to include in the SPC file.</span></span> <span data-ttu-id="6ddc9-117">可以指定用空格分隔的多个名称。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-117">You can specify multiple names separated by spaces.</span></span>|  
|`crlN.crl`|<span data-ttu-id="6ddc9-118">要包含在 SPC 文件中的证书吊销列表的名称。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-118">The name of a certificate revocation list to include in the SPC file.</span></span> <span data-ttu-id="6ddc9-119">可以指定用空格分隔的多个名称。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-119">You can specify multiple names separated by spaces.</span></span>|  
|`outputSPCfile.spc`|<span data-ttu-id="6ddc9-120">将包含 X.509 证书的 PKCS #7 对象的名称。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-120">The name of the PKCS #7 object that will contain the X.509 certificates.</span></span>|  
  
|<span data-ttu-id="6ddc9-121">选项</span><span class="sxs-lookup"><span data-stu-id="6ddc9-121">Option</span></span>|<span data-ttu-id="6ddc9-122">说明</span><span class="sxs-lookup"><span data-stu-id="6ddc9-122">Description</span></span>|  
|------------|-----------------|  
|<span data-ttu-id="6ddc9-123">**/?**</span><span class="sxs-lookup"><span data-stu-id="6ddc9-123">**/?**</span></span>|<span data-ttu-id="6ddc9-124">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-124">Displays command syntax and options for the tool.</span></span>|  
  
## <a name="examples"></a><span data-ttu-id="6ddc9-125">示例</span><span class="sxs-lookup"><span data-stu-id="6ddc9-125">Examples</span></span>  

 <span data-ttu-id="6ddc9-126">下面的命令从 `myCertificate.cer` 创建一个 SPC 并将其放入 `mySPCFile.spc`。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-126">The following command creates an SPC from `myCertificate.cer` and places it in `mySPCFile.spc`.</span></span>  
  
```console
cert2spc myCertificate.cer mySPCFile.spc  
```  
  
 <span data-ttu-id="6ddc9-127">下面的命令从 `oneCertificate.cer` 和 `twoCertificate.cer` 创建一个 SPC，并将其放入 `mySPCFile.spc`。</span><span class="sxs-lookup"><span data-stu-id="6ddc9-127">The following command creates an SPC from `oneCertificate.cer` and `twoCertificate.cer`, and places it in `mySPCFile.spc`.</span></span>  
  
```console
cert2spc oneCertificate.cer twoCertificate.cer mySPCFile.spc  
```  
  
## <a name="see-also"></a><span data-ttu-id="6ddc9-128">请参阅</span><span class="sxs-lookup"><span data-stu-id="6ddc9-128">See also</span></span>

- [<span data-ttu-id="6ddc9-129">工具</span><span class="sxs-lookup"><span data-stu-id="6ddc9-129">Tools</span></span>](index.md)
- [<span data-ttu-id="6ddc9-130">Makecert.exe（证书创建工具）</span><span class="sxs-lookup"><span data-stu-id="6ddc9-130">Makecert.exe (Certificate Creation Tool)</span></span>](/windows/desktop/SecCrypto/makecert)
- [<span data-ttu-id="6ddc9-131">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="6ddc9-131">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)

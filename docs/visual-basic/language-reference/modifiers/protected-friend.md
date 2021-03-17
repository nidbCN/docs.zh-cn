---
description: '了解有关以下内容的详细信息：受保护的 Friend (Visual Basic) '
title: Protected Friend
ms.date: 05/10/2018
f1_keywords:
- vb.ProtectedFriend
helpviewer_keywords:
- Protected Friend keyword [Visual Basic]
- Protected Friend keyword [Visual Basic], syntax
ms.openlocfilehash: b92f52345d5b4b2b67745da8f142cd1484aec588
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624001"
---
# <a name="protected-friend-visual-basic"></a><span data-ttu-id="49105-103">受保护的 Friend (Visual Basic) </span><span class="sxs-lookup"><span data-stu-id="49105-103">Protected Friend (Visual Basic)</span></span>

<span data-ttu-id="49105-104">`Protected Friend` 关键字组合是一种成员访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="49105-104">The `Protected Friend` keyword combination is a member access modifier.</span></span> <span data-ttu-id="49105-105">它在已声明的元素上授予 [朋友](friend.md) 访问和 [受保护](protected.md) 的访问，因此可以从同一程序集中的任何位置、从其自己的类和派生类中访问它们。</span><span class="sxs-lookup"><span data-stu-id="49105-105">It confers both [Friend](friend.md) access and [Protected](protected.md) access on the declared elements, so they are accessible from anywhere in the same assembly, from their own class, and from derived classes.</span></span> <span data-ttu-id="49105-106">只能 `Protected Friend` 在类的成员上指定; 无法应用 `Protected Friend` 于结构的成员，因为结构不能继承。</span><span class="sxs-lookup"><span data-stu-id="49105-106">You can specify `Protected Friend` only on members of classes; you cannot apply `Protected Friend` to members of a structure because structures cannot be inherited.</span></span>

> [!NOTE]
> <span data-ttu-id="49105-107">在 Visual Studio 中，选择 "F1 帮助" `Protected Friend` 可为 [受保护](protected.md) 的或 [朋友](friend.md)提供帮助。</span><span class="sxs-lookup"><span data-stu-id="49105-107">In Visual Studio, selecting F1 help on `Protected Friend` provides help for either [Protected](protected.md) or [Friend](friend.md).</span></span> <span data-ttu-id="49105-108">IDE 将选取光标下的单个标记，而不是组合词。</span><span class="sxs-lookup"><span data-stu-id="49105-108">The IDE picks the single token under the cursor rather than the compound word.</span></span>

## <a name="see-also"></a><span data-ttu-id="49105-109">另请参阅</span><span class="sxs-lookup"><span data-stu-id="49105-109">See also</span></span>

- [<span data-ttu-id="49105-110">Public</span><span class="sxs-lookup"><span data-stu-id="49105-110">Public</span></span>](public.md)
- [<span data-ttu-id="49105-111">避免</span><span class="sxs-lookup"><span data-stu-id="49105-111">Protected</span></span>](protected.md)
- [<span data-ttu-id="49105-112">友好</span><span class="sxs-lookup"><span data-stu-id="49105-112">Friend</span></span>](friend.md)
- [<span data-ttu-id="49105-113">专用</span><span class="sxs-lookup"><span data-stu-id="49105-113">Private</span></span>](private.md)
- [<span data-ttu-id="49105-114">私有受保护</span><span class="sxs-lookup"><span data-stu-id="49105-114">Private Protected</span></span>](./private-protected.md)
- [<span data-ttu-id="49105-115">Visual Basic 中的访问级别</span><span class="sxs-lookup"><span data-stu-id="49105-115">Access levels in Visual Basic</span></span>](../../programming-guide/language-features/declared-elements/access-levels.md)
- [<span data-ttu-id="49105-116">过程</span><span class="sxs-lookup"><span data-stu-id="49105-116">Procedures</span></span>](../../programming-guide/language-features/procedures/index.md)
- [<span data-ttu-id="49105-117">结构</span><span class="sxs-lookup"><span data-stu-id="49105-117">Structures</span></span>](../../programming-guide/language-features/data-types/structures.md)
- [<span data-ttu-id="49105-118">对象和类</span><span class="sxs-lookup"><span data-stu-id="49105-118">Objects and Classes</span></span>](../../programming-guide/language-features/objects-and-classes/index.md)

---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347894"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="38fc2-101">このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する Bot フレームワーク bot を構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="38fc2-101">This tutorial teaches you how to build a Bot Framework bot that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="38fc2-102">完成したチュートリアルをダウンロードするだけで済む場合は、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-botframework)をダウンロードするか、クローンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="38fc2-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-botframework).</span></span> <span data-ttu-id="38fc2-103">アプリ ID と secret を使用してアプリを構成する方法については、 **demo** フォルダーの README ファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="38fc2-103">See the README file in the **demo** folder for instructions on configuring the app with an app ID and secret.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38fc2-104">前提条件</span><span class="sxs-lookup"><span data-stu-id="38fc2-104">Prerequisites</span></span>

<span data-ttu-id="38fc2-105">このチュートリアルを開始する前に、開発用のコンピューターに次のものがインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="38fc2-105">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="38fc2-106">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="38fc2-106">.NET Core SDK</span></span>](https://dotnet.microsoft.com/download)
- [<span data-ttu-id="38fc2-107">Bot フレームワークエミュレーター</span><span class="sxs-lookup"><span data-stu-id="38fc2-107">Bot Framework Emulator</span></span>](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

<span data-ttu-id="38fc2-108">また、Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントを所有している必要があります。</span><span class="sxs-lookup"><span data-stu-id="38fc2-108">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="38fc2-109">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="38fc2-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="38fc2-110">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="38fc2-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="38fc2-111">[Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="38fc2-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>
- <span data-ttu-id="38fc2-112">Azure サブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="38fc2-112">An Azure subscription.</span></span> <span data-ttu-id="38fc2-113">使用していない場合は、開始する前に、 [無料のアカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成します。</span><span class="sxs-lookup"><span data-stu-id="38fc2-113">If you do not have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

> [!NOTE]
> <span data-ttu-id="38fc2-114">このチュートリアルは、.NET Core SDK version 3.1.401 を使用して作成されています。</span><span class="sxs-lookup"><span data-stu-id="38fc2-114">This tutorial was written with .NET Core SDK version 3.1.401.</span></span> <span data-ttu-id="38fc2-115">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。</span><span class="sxs-lookup"><span data-stu-id="38fc2-115">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="38fc2-116">フィードバック</span><span class="sxs-lookup"><span data-stu-id="38fc2-116">Feedback</span></span>

<span data-ttu-id="38fc2-117">このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-botframework)に記入してください。</span><span class="sxs-lookup"><span data-stu-id="38fc2-117">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-botframework).</span></span>

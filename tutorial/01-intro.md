---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347894"
---
<!-- markdownlint-disable MD002 MD041 -->

このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する Bot フレームワーク bot を構築する方法について説明します。

> [!TIP]
> 完成したチュートリアルをダウンロードするだけで済む場合は、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-botframework)をダウンロードするか、クローンを作成できます。 アプリ ID と secret を使用してアプリを構成する方法については、 **demo** フォルダーの README ファイルを参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルを開始する前に、開発用のコンピューターに次のものがインストールされている必要があります。

- [.NET Core SDK](https://dotnet.microsoft.com/download)
- [Bot フレームワークエミュレーター](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

また、Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントを所有している必要があります。 Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。

- [新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。
- [Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。
- Azure サブスクリプション。 使用していない場合は、開始する前に、 [無料のアカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成します。

> [!NOTE]
> このチュートリアルは、.NET Core SDK version 3.1.401 を使用して作成されています。 このガイドの手順は、他のバージョンでは動作しますが、テストされていません。

## <a name="feedback"></a>フィードバック

このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-botframework)に記入してください。

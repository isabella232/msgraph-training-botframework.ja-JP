---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347897"
---
# <a name="graphcalendarbot"></a>GraphCalendarBot

Bot フレームワーク v4 echo bot のサンプル。

この bot は [Bot フレームワーク](https://dev.botframework.com)を使用して作成されています。これは、ユーザーからの入力を受け付けてエコーバックする簡単な bot を作成する方法を示しています。

## <a name="prerequisites"></a>前提条件

- [.Net コア SDK](https://dotnet.microsoft.com/download) バージョン3.1

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a>このサンプルを実行するには

- ターミナルで、 `GraphCalendarBot`

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- 端末または Visual Studio から bot を実行し、オプション A または B を選択します。

  A) ターミナル

  ```bash
  # run the bot
  dotnet run
  ```

  B) または Visual Studio

  - Visual Studio の起動
  - ファイル-> オープン > プロジェクト/ソリューション
  - フォルダーに移動します。 `GraphCalendarBot`
  - ファイルの選択 `GraphCalendarBot.csproj`
  - を押して `F5` プロジェクトを実行します。

## <a name="testing-the-bot-using-bot-framework-emulator"></a>Bot フレームワークエミュレーターを使用した bot のテスト

[Bot フレームワークエミュレーター](https://github.com/microsoft/botframework-emulator) は、ボット開発者が localhost で bot をテストしてデバッグできるようにするデスクトップアプリケーションです。また、トンネルを介してリモートで実行することもできます。

- [ここ](https://github.com/Microsoft/BotFramework-Emulator/releases)から4.9.0 またはそれ以上のボットフレームワークエミュレーターバージョンをインストールします。

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a>Bot フレームワークエミュレーターを使用して bot に接続する

- Bot フレームワークエミュレーターを起動する
- ファイル-> Open Bot
- の Bot URL を入力してください `http://localhost:3978/api/messages`

## <a name="deploy-the-bot-to-azure"></a>Bot を Azure に展開する

Bot を Azure に展開する方法の詳細については、「展開手順の完全な一覧については、「 [azure に bot を展開する](https://aka.ms/azuredeployment) 」を参照してください。

## <a name="further-reading"></a>参考資料

- [Bot フレームワークのドキュメント](https://docs.botframework.com)
- [Bot の基本](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [アクティビティ処理](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [Azure Bot サービスの概要](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Azure Bot サービスのドキュメント](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [.NET コア CLI ツール](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Azure Portal](https://portal.azure.com)
- [LUIS を使用した言語の理解](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [チャネルおよび Bot コネクタサービス](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

V 4.10.2 を使用して生成された `dotnet new echobot`

---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347897"
---
# <a name="graphcalendarbot"></a><span data-ttu-id="f6a9d-101">GraphCalendarBot</span><span class="sxs-lookup"><span data-stu-id="f6a9d-101">GraphCalendarBot</span></span>

<span data-ttu-id="f6a9d-102">Bot フレームワーク v4 echo bot のサンプル。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-102">Bot Framework v4 echo bot sample.</span></span>

<span data-ttu-id="f6a9d-103">この bot は [Bot フレームワーク](https://dev.botframework.com)を使用して作成されています。これは、ユーザーからの入力を受け付けてエコーバックする簡単な bot を作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-103">This bot has been created using [Bot Framework](https://dev.botframework.com), it shows how to create a simple bot that accepts input from the user and echoes it back.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6a9d-104">前提条件</span><span class="sxs-lookup"><span data-stu-id="f6a9d-104">Prerequisites</span></span>

- <span data-ttu-id="f6a9d-105">[.Net コア SDK](https://dotnet.microsoft.com/download) バージョン3.1</span><span class="sxs-lookup"><span data-stu-id="f6a9d-105">[.NET Core SDK](https://dotnet.microsoft.com/download) version 3.1</span></span>

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a><span data-ttu-id="f6a9d-106">このサンプルを実行するには</span><span class="sxs-lookup"><span data-stu-id="f6a9d-106">To try this sample</span></span>

- <span data-ttu-id="f6a9d-107">ターミナルで、 `GraphCalendarBot`</span><span class="sxs-lookup"><span data-stu-id="f6a9d-107">In a terminal, navigate to `GraphCalendarBot`</span></span>

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- <span data-ttu-id="f6a9d-108">端末または Visual Studio から bot を実行し、オプション A または B を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-108">Run the bot from a terminal or from Visual Studio, choose option A or B.</span></span>

  <span data-ttu-id="f6a9d-109">A) ターミナル</span><span class="sxs-lookup"><span data-stu-id="f6a9d-109">A) From a terminal</span></span>

  ```bash
  # run the bot
  dotnet run
  ```

  <span data-ttu-id="f6a9d-110">B) または Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f6a9d-110">B) Or from Visual Studio</span></span>

  - <span data-ttu-id="f6a9d-111">Visual Studio の起動</span><span class="sxs-lookup"><span data-stu-id="f6a9d-111">Launch Visual Studio</span></span>
  - <span data-ttu-id="f6a9d-112">ファイル-> オープン > プロジェクト/ソリューション</span><span class="sxs-lookup"><span data-stu-id="f6a9d-112">File -> Open -> Project/Solution</span></span>
  - <span data-ttu-id="f6a9d-113">フォルダーに移動します。 `GraphCalendarBot`</span><span class="sxs-lookup"><span data-stu-id="f6a9d-113">Navigate to `GraphCalendarBot` folder</span></span>
  - <span data-ttu-id="f6a9d-114">ファイルの選択 `GraphCalendarBot.csproj`</span><span class="sxs-lookup"><span data-stu-id="f6a9d-114">Select `GraphCalendarBot.csproj` file</span></span>
  - <span data-ttu-id="f6a9d-115">を押して `F5` プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-115">Press `F5` to run the project</span></span>

## <a name="testing-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="f6a9d-116">Bot フレームワークエミュレーターを使用した bot のテスト</span><span class="sxs-lookup"><span data-stu-id="f6a9d-116">Testing the bot using Bot Framework Emulator</span></span>

<span data-ttu-id="f6a9d-117">[Bot フレームワークエミュレーター](https://github.com/microsoft/botframework-emulator) は、ボット開発者が localhost で bot をテストしてデバッグできるようにするデスクトップアプリケーションです。また、トンネルを介してリモートで実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-117">[Bot Framework Emulator](https://github.com/microsoft/botframework-emulator) is a desktop application that allows bot developers to test and debug their bots on localhost or running remotely through a tunnel.</span></span>

- <span data-ttu-id="f6a9d-118">[ここ](https://github.com/Microsoft/BotFramework-Emulator/releases)から4.9.0 またはそれ以上のボットフレームワークエミュレーターバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-118">Install the Bot Framework Emulator version 4.9.0 or greater from [here](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="f6a9d-119">Bot フレームワークエミュレーターを使用して bot に接続する</span><span class="sxs-lookup"><span data-stu-id="f6a9d-119">Connect to the bot using Bot Framework Emulator</span></span>

- <span data-ttu-id="f6a9d-120">Bot フレームワークエミュレーターを起動する</span><span class="sxs-lookup"><span data-stu-id="f6a9d-120">Launch Bot Framework Emulator</span></span>
- <span data-ttu-id="f6a9d-121">ファイル-> Open Bot</span><span class="sxs-lookup"><span data-stu-id="f6a9d-121">File -> Open Bot</span></span>
- <span data-ttu-id="f6a9d-122">の Bot URL を入力してください `http://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="f6a9d-122">Enter a Bot URL of `http://localhost:3978/api/messages`</span></span>

## <a name="deploy-the-bot-to-azure"></a><span data-ttu-id="f6a9d-123">Bot を Azure に展開する</span><span class="sxs-lookup"><span data-stu-id="f6a9d-123">Deploy the bot to Azure</span></span>

<span data-ttu-id="f6a9d-124">Bot を Azure に展開する方法の詳細については、「展開手順の完全な一覧については、「 [azure に bot を展開する](https://aka.ms/azuredeployment) 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6a9d-124">To learn more about deploying a bot to Azure, see [Deploy your bot to Azure](https://aka.ms/azuredeployment) for a complete list of deployment instructions.</span></span>

## <a name="further-reading"></a><span data-ttu-id="f6a9d-125">参考資料</span><span class="sxs-lookup"><span data-stu-id="f6a9d-125">Further reading</span></span>

- [<span data-ttu-id="f6a9d-126">Bot フレームワークのドキュメント</span><span class="sxs-lookup"><span data-stu-id="f6a9d-126">Bot Framework Documentation</span></span>](https://docs.botframework.com)
- [<span data-ttu-id="f6a9d-127">Bot の基本</span><span class="sxs-lookup"><span data-stu-id="f6a9d-127">Bot Basics</span></span>](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [<span data-ttu-id="f6a9d-128">アクティビティ処理</span><span class="sxs-lookup"><span data-stu-id="f6a9d-128">Activity processing</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [<span data-ttu-id="f6a9d-129">Azure Bot サービスの概要</span><span class="sxs-lookup"><span data-stu-id="f6a9d-129">Azure Bot Service Introduction</span></span>](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [<span data-ttu-id="f6a9d-130">Azure Bot サービスのドキュメント</span><span class="sxs-lookup"><span data-stu-id="f6a9d-130">Azure Bot Service Documentation</span></span>](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [<span data-ttu-id="f6a9d-131">.NET コア CLI ツール</span><span class="sxs-lookup"><span data-stu-id="f6a9d-131">.NET Core CLI tools</span></span>](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [<span data-ttu-id="f6a9d-132">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f6a9d-132">Azure CLI</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [<span data-ttu-id="f6a9d-133">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f6a9d-133">Azure Portal</span></span>](https://portal.azure.com)
- [<span data-ttu-id="f6a9d-134">LUIS を使用した言語の理解</span><span class="sxs-lookup"><span data-stu-id="f6a9d-134">Language Understanding using LUIS</span></span>](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [<span data-ttu-id="f6a9d-135">チャネルおよび Bot コネクタサービス</span><span class="sxs-lookup"><span data-stu-id="f6a9d-135">Channels and Bot Connector Service</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

<span data-ttu-id="f6a9d-136">V 4.10.2 を使用して生成された `dotnet new echobot`</span><span class="sxs-lookup"><span data-stu-id="f6a9d-136">Generated using `dotnet new echobot` v4.10.2</span></span>

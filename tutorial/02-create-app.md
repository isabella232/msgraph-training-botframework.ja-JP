---
ms.openlocfilehash: 12a6c18b52e2bc9aba5ad69c8bb9ab8091c11a64
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347888"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、ボットフレームワークプロジェクトを作成します。

1. プロジェクトを作成するディレクトリで、コマンドラインインターフェイス (CLI) を開きます。 次のコマンドを実行して、 **EchoBot** テンプレートを使用して新しいプロジェクトを作成します。

    ```dotnetcli
    dotnet new echobot -n GraphCalendarBot
    ```

    > [!NOTE]
    > エラーが表示された場合は、 `No templates matched the input template name: echobot.` 次のコマンドを使用してテンプレートをインストールし、前のコマンドを再度実行します。
    >
    > ```dotnetcli
    > dotnet new -i Microsoft.Bot.Framework.CSharp.EchoBot
    > ```

1. 既定の **EchoBot** クラスの名前を **calendarbot** に変更します。 **/Bots/EchoBot.cs** を開き、のすべてのインスタンスを置き換え `EchoBot` `CalendarBot` ます。 ファイルの名前を **CalendarBot.cs** に変更します。

1. `EchoBot` `CalendarBot` その他の **.cs** ファイルで、のすべてのインスタンスを置換します。

1. CLI で、現在のディレクトリを **Graphcalendarbot** ディレクトリに変更し、次のコマンドを実行してプロジェクトのビルドを確認します。

    ```dotnetcli
    dotnet build
    ```

## <a name="add-nuget-packages"></a>NuGet パッケージを追加する

に進む前に、後で使用する追加の NuGet パッケージをインストールします。

- [AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) が、応答でアダプティブカードを送信することを許可します。
- Bot にダイアログのサポートを[追加するためのダイアログ。](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/)
- Bot プロンプトから返される TIMEX 式を **DateTime** オブジェクトに変換するための、 [Microsoft レコグナイザー exexpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) 。
- [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/): Microsoft Graph を呼び出すためのものです。

1. CLI で次のコマンドを実行して、依存関係をインストールします。

    ```Shell
    dotnet add package AdaptiveCards --version 2.2.0
    dotnet add package Microsoft.Bot.Builder.Dialogs --version 4.10.3
    dotnet add package Microsoft.Bot.Builder.Integration.AspNet.Core --version 4.10.3
    dotnet add package Microsoft.Recognizers.Text.DataTypes.TimexExpression --version 1.4.1
    dotnet add package Microsoft.Graph --version 3.18.0
    ```

## <a name="test-the-bot"></a>Bot をテストする

コードを追加する前に、bot をテストして正常に動作することを確認し、Bot フレームワークエミュレーターがテストするように構成されていることを確認します。

1. 次のコマンドを実行して、ボットを開始します。

    ```dotnetcli
    dotnet run
    ```

    > [!TIP]
    > プロジェクト内のソースファイルは、任意のテキストエディターを使用して編集できますが、 [Visual Studio Code](https://code.visualstudio.com/)を使用することをお勧めします。 Visual Studio Code では、デバッグサポート、Intellisense などが提供されています。 Visual Studio Code を使用している **場合は、[**  ->  **デバッグ開始**] メニューを使用して bot を開始できます。

1. Bot が実行中であることを確認するには、ブラウザーを開いてに進み `http://localhost:3978` ます。 Bot の準備ができたことが表示 **されます。** メッセージ。

1. Bot フレームワークエミュレーターを開きます。 [ **ファイル** ] メニューを選択し、[Bot] を **開き** ます。

1. `http://localhost:3978/api/messages` **BOT の URL** にを入力し、[**接続**] を選択します。

1. Bot が `Hello and welcome!` チャットウィンドウで応答します。 Bot にメッセージを送信して、それがエコーバックすることを確認します。

    ![Bot に接続された Bot フレームワークエミュレーターのスクリーンショット](images/test-emulator.png)

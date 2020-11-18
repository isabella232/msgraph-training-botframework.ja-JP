---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347849"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、Microsoft Graph SDK を使用して、ユーザーの予定表にイベントを追加します。

## <a name="implement-a-dialog"></a>ダイアログを実装する

最初に、予定表にイベントを追加するために必要な値をユーザーに確認するための新しいカスタムダイアログを作成します。 このダイアログでは、 **WaterfallDialog** を使用して次の手順を実行します。

- 件名の入力を求める
- ユーザーが自分の招待を希望するかどうかを確認する
- 出席者を確認する (ユーザーが前の手順に出た場合)
- [開始日時の入力を求める]
- 終了日と終了時刻の入力を求める
- 収集されたすべての値を表示して、ユーザーに確認を求める
- ユーザーが確認した場合は、 **Oauthprompt** からアクセストークンを取得します。
- イベントを作成する

1. **NewEventDialog.cs** という名前のフォルダーに新しいファイルを作成し、次のコードを追加します **。**

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;
    using CalendarBot.Graph;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    using TimexTypes = Microsoft.Recognizers.Text.DataTypes.TimexExpression.Constants.TimexTypes;

    namespace CalendarBot.Dialogs
    {
        public class NewEventDialog : LogoutDialog
        {
            protected readonly ILogger _logger;
            private readonly IGraphClientService _graphClientService;

            public NewEventDialog(
                IConfiguration configuration,
                IGraphClientService graphClientService)
                : base(nameof(NewEventDialog), configuration["ConnectionName"])
            {

            }

            // Generate a DateTime from the list of
            // DateTimeResolutions provided by the DateTimePrompt
            private static DateTime GetDateTimeFromResolutions(IList<DateTimeResolution> resolutions)
            {
                var timex = new TimexProperty(resolutions[0].Timex);

                // Handle the "now" case
                if (timex.Now ?? false)
                {
                    return DateTime.Now;
                }

                // Otherwise generate a DateTime
                return TimexHelpers.DateFromTimex(timex);
            }
        }
    }
    ```

    これは、ユーザーに予定表にイベントを追加するために必要な値を求めるダイアログを表示する新しいダイアログのシェルです。

1. **NewEventDialog** クラスに次の関数を追加して、件名の入力をユーザーに求めます。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、ユーザーが前の手順で指定した件名を格納し、出席者を追加するかどうかを確認します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、前の手順でユーザーの応答を確認し、必要に応じてユーザーに出席者の一覧を要求します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、前の手順で出席者のリストを保存します (存在する場合)。また、開始日時の入力を求めるメッセージをユーザーに表示します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、前の手順の開始値を格納し、終了日時を指定するようユーザーに求めるメッセージを表示します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. 前の手順の最後の値を格納するために、次の関数を **NewEventDialog** クラスに追加し、ユーザーにすべての入力を確認するように求めます。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. 前の手順でユーザーの応答を確認するには、 **NewEventDialog** クラスに次の関数を追加します。 ユーザーが入力を確認した場合は、 **Oauthprompt** クラスを使用してアクセストークンを取得します。 それ以外の場合は、ダイアログを終了します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、MICROSOFT Graph SDK を使用して新しいイベントを作成します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    このコードの内容を検討してください。

    - ユーザーの優先タイムゾーンを決定するために、ユーザーの **MailboxSettings** を取得します。
    - ユーザーによって提供される値を使用して、 **Event** オブジェクトを作成します。 **Start** および **End** プロパティは、ユーザーのタイムゾーンで設定されることに注意してください。
    - ユーザーの予定表にイベントを作成します。

## <a name="add-validation"></a>検証を追加する

次に、Microsoft Graph でイベントを作成するときのエラーを回避するために、ユーザーの入力に検証を追加します。 次のことを確認してください。

- ユーザーが出席者リストを指定する場合は、有効な電子メールアドレスをセミコロンで区切ったリストにする必要があります。
- 開始日時は、有効な日付と時刻である必要があります。
- 終了日時は、有効な日付と時刻である必要があり、開始日よりも後にする必要があります。

1. **NewEventDialog** クラスに次の関数を追加して、ユーザーの出席者の入力を検証します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、ユーザーの開始日時の入力を検証します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. **NewEventDialog** クラスに次の関数を追加して、ユーザーの終了日時の入力を検証します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a>WaterfallDialog に手順を追加する

これで、ダイアログのすべての "手順" が完了したので、最後の手順として、それらをコンストラクターの **WaterfallDialog** に追加して、 **NewEventDialog** を **maindialog** に追加します。

1. **NewEventDialog** コンストラクタを見つけて、次のコードを追加します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    これにより、使用されているすべてのダイアログが追加され、 **WaterfallDialog** の手順として実装したすべての関数が追加されます。

1. [] を開き、次の行をコンストラクターに追加し **ます。**

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. ブロック内のコードを `else if (command.StartsWith("add event"))` 次のように置き換え `ProcessStepAsync` ます。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. すべての変更を保存し、bot を再起動します。

1. Bot フレームワークエミュレーターを使用して bot に接続し、ログインします。 [ **Add event** ] ボタンを選択します。

1. プロンプトに応答して、新しいイベントを作成します。 Start および end の値を入力するように求められた場合は、"today at 3PM" または "now" のような語句を使用することに注意してください。

    ![Bot フレームワークエミュレーターの [イベントの追加] ダイアログのスクリーンショット](images/add-event.png)

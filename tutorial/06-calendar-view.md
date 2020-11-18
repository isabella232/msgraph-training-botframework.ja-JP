---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347924"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、Microsoft Graph SDK を使用して、現在の週について、ユーザーの予定表にある次の3つのイベントを取得します。

## <a name="get-a-calendar-view"></a>予定表ビューを取得する

予定表ビューは、ユーザーの予定表にある、2つの日付/時刻の値の間にあるイベントの一覧です。 予定表ビューを使用する利点は、定期的な会議がすべて含まれていることです。

1. CardHelper.cs を開き、次の関数を **Cardhelper** クラスに追加します **。**

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    このコードでは、予定表イベントをレンダリングするためのアダプティブカードを作成します。

1. を開いて、次の関数を **Maindialog** クラスに追加し **ます。**

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    このコードの内容を検討してください。

    - ユーザーの優先タイムゾーンおよび日付と時刻の形式を決定するために、ユーザーの **MailboxSettings** を取得します。
    - この例では、 **startDateTime** と **enddatetime** の値をそれぞれ、週の終わりに設定します。 これにより、予定表ビューが使用する時間のウィンドウが定義されます。
    - このメソッド `graphClient.Me.CalendarView` は、次の詳細情報を呼び出します。
        - `Prefer: outlook.timezone`ユーザーの優先タイムゾーンにヘッダーを設定することにより、イベントの開始時刻と終了時刻がユーザーのタイムゾーンに含まれます。
        - メソッドを使用して、 `Top(3)` 結果を最初の3つのイベントのみに制限します。
        - を使用 `Select` して、bot が使用するフィールドのみに返されるフィールドを制限します。
        - このメソッドは `OrderBy` 、イベントを開始時刻で並べ替えるために使用します。
    - 応答メッセージには、各イベントのアダプティブカードが追加されます。

1. ブロック内のコードを `else if (command.StartsWith("show calendar"))` 次のように置き換え `ProcessStepAsync` ます。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. すべての変更を保存し、bot を再起動します。

1. Bot フレームワークエミュレーターを使用して bot に接続し、ログインします。 [ **予定表の表示** ] ボタンを選択して、予定表ビューを表示します。

    ![次の3つのイベントを示すアダプティブカードのスクリーンショット](images/calendar-view.png)

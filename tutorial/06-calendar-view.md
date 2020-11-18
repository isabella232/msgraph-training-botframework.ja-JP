---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347924"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="63075-101">このセクションでは、Microsoft Graph SDK を使用して、現在の週について、ユーザーの予定表にある次の3つのイベントを取得します。</span><span class="sxs-lookup"><span data-stu-id="63075-101">In this section you'll use the Microsoft Graph SDK to get the next 3 upcoming events on the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="63075-102">予定表ビューを取得する</span><span class="sxs-lookup"><span data-stu-id="63075-102">Get a calendar view</span></span>

<span data-ttu-id="63075-103">予定表ビューは、ユーザーの予定表にある、2つの日付/時刻の値の間にあるイベントの一覧です。</span><span class="sxs-lookup"><span data-stu-id="63075-103">A calendar view is a list of events on a user's calendar that fall between two date/time values.</span></span> <span data-ttu-id="63075-104">予定表ビューを使用する利点は、定期的な会議がすべて含まれていることです。</span><span class="sxs-lookup"><span data-stu-id="63075-104">The advantage of using a calendar view is that it includes any occurrences of recurring meetings.</span></span>

1. <span data-ttu-id="63075-105">CardHelper.cs を開き、次の関数を **Cardhelper** クラスに追加します **。**</span><span class="sxs-lookup"><span data-stu-id="63075-105">Open **./CardHelper.cs** and add the following function to the **CardHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    <span data-ttu-id="63075-106">このコードでは、予定表イベントをレンダリングするためのアダプティブカードを作成します。</span><span class="sxs-lookup"><span data-stu-id="63075-106">This code builds an Adaptive Card to render a calendar event.</span></span>

1. <span data-ttu-id="63075-107">を開いて、次の関数を **Maindialog** クラスに追加し **ます。**</span><span class="sxs-lookup"><span data-stu-id="63075-107">Open **./Dialogs/MainDialog.cs** and add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    <span data-ttu-id="63075-108">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="63075-108">Consider what this code does.</span></span>

    - <span data-ttu-id="63075-109">ユーザーの優先タイムゾーンおよび日付と時刻の形式を決定するために、ユーザーの **MailboxSettings** を取得します。</span><span class="sxs-lookup"><span data-stu-id="63075-109">It gets the user's **MailboxSettings** to determine the user's preferred time zone and date/time formats.</span></span>
    - <span data-ttu-id="63075-110">この例では、 **startDateTime** と **enddatetime** の値をそれぞれ、週の終わりに設定します。</span><span class="sxs-lookup"><span data-stu-id="63075-110">It sets the **startDateTime** and **endDateTime** values to now and the end of the week, respectively.</span></span> <span data-ttu-id="63075-111">これにより、予定表ビューが使用する時間のウィンドウが定義されます。</span><span class="sxs-lookup"><span data-stu-id="63075-111">This defines the window of time that the calendar view uses.</span></span>
    - <span data-ttu-id="63075-112">このメソッド `graphClient.Me.CalendarView` は、次の詳細情報を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="63075-112">It calls `graphClient.Me.CalendarView` with the following details.</span></span>
        - <span data-ttu-id="63075-113">`Prefer: outlook.timezone`ユーザーの優先タイムゾーンにヘッダーを設定することにより、イベントの開始時刻と終了時刻がユーザーのタイムゾーンに含まれます。</span><span class="sxs-lookup"><span data-stu-id="63075-113">It sets the `Prefer: outlook.timezone` header to the user's preferred time zone, causing the start and end times for the events to be in the user's timezone.</span></span>
        - <span data-ttu-id="63075-114">メソッドを使用して、 `Top(3)` 結果を最初の3つのイベントのみに制限します。</span><span class="sxs-lookup"><span data-stu-id="63075-114">It uses the `Top(3)` method to limit the results to only the first 3 events.</span></span>
        - <span data-ttu-id="63075-115">を使用 `Select` して、bot が使用するフィールドのみに返されるフィールドを制限します。</span><span class="sxs-lookup"><span data-stu-id="63075-115">It uses `Select` to limit the fields returned to just those fields used by the bot.</span></span>
        - <span data-ttu-id="63075-116">このメソッドは `OrderBy` 、イベントを開始時刻で並べ替えるために使用します。</span><span class="sxs-lookup"><span data-stu-id="63075-116">It uses `OrderBy` to sort the events by start time.</span></span>
    - <span data-ttu-id="63075-117">応答メッセージには、各イベントのアダプティブカードが追加されます。</span><span class="sxs-lookup"><span data-stu-id="63075-117">It adds an Adaptive Card for each event to the reply message.</span></span>

1. <span data-ttu-id="63075-118">ブロック内のコードを `else if (command.StartsWith("show calendar"))` 次のように置き換え `ProcessStepAsync` ます。</span><span class="sxs-lookup"><span data-stu-id="63075-118">Replace the code inside the `else if (command.StartsWith("show calendar"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. <span data-ttu-id="63075-119">すべての変更を保存し、bot を再起動します。</span><span class="sxs-lookup"><span data-stu-id="63075-119">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="63075-120">Bot フレームワークエミュレーターを使用して bot に接続し、ログインします。</span><span class="sxs-lookup"><span data-stu-id="63075-120">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="63075-121">[ **予定表の表示** ] ボタンを選択して、予定表ビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="63075-121">Select the **Show calendar** button to display the calendar view.</span></span>

    ![次の3つのイベントを示すアダプティブカードのスクリーンショット](images/calendar-view.png)

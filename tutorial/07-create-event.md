---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347849"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="56385-101">このセクションでは、Microsoft Graph SDK を使用して、ユーザーの予定表にイベントを追加します。</span><span class="sxs-lookup"><span data-stu-id="56385-101">In this section you'll use the Microsoft Graph SDK to add an event to the user's calendar.</span></span>

## <a name="implement-a-dialog"></a><span data-ttu-id="56385-102">ダイアログを実装する</span><span class="sxs-lookup"><span data-stu-id="56385-102">Implement a dialog</span></span>

<span data-ttu-id="56385-103">最初に、予定表にイベントを追加するために必要な値をユーザーに確認するための新しいカスタムダイアログを作成します。</span><span class="sxs-lookup"><span data-stu-id="56385-103">Start by creating a new custom dialog to prompt the user for the values needed to add an event to their calendar.</span></span> <span data-ttu-id="56385-104">このダイアログでは、 **WaterfallDialog** を使用して次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="56385-104">This dialog will use a **WaterfallDialog** to do the following steps.</span></span>

- <span data-ttu-id="56385-105">件名の入力を求める</span><span class="sxs-lookup"><span data-stu-id="56385-105">Prompt for a subject</span></span>
- <span data-ttu-id="56385-106">ユーザーが自分の招待を希望するかどうかを確認する</span><span class="sxs-lookup"><span data-stu-id="56385-106">Ask if the user wants to invite people</span></span>
- <span data-ttu-id="56385-107">出席者を確認する (ユーザーが前の手順に出た場合)</span><span class="sxs-lookup"><span data-stu-id="56385-107">Prompt for attendees (if user said yes to previous step)</span></span>
- <span data-ttu-id="56385-108">[開始日時の入力を求める]</span><span class="sxs-lookup"><span data-stu-id="56385-108">Prompt for a start date and time</span></span>
- <span data-ttu-id="56385-109">終了日と終了時刻の入力を求める</span><span class="sxs-lookup"><span data-stu-id="56385-109">Prompt for an end date and time</span></span>
- <span data-ttu-id="56385-110">収集されたすべての値を表示して、ユーザーに確認を求める</span><span class="sxs-lookup"><span data-stu-id="56385-110">Display all of the collected values and ask user to confirm</span></span>
- <span data-ttu-id="56385-111">ユーザーが確認した場合は、 **Oauthprompt** からアクセストークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="56385-111">If user confirms, get access token from **OAuthPrompt**</span></span>
- <span data-ttu-id="56385-112">イベントを作成する</span><span class="sxs-lookup"><span data-stu-id="56385-112">Create the event</span></span>

1. <span data-ttu-id="56385-113">**NewEventDialog.cs** という名前のフォルダーに新しいファイルを作成し、次のコードを追加します **。**</span><span class="sxs-lookup"><span data-stu-id="56385-113">Create a new file in the **./Dialogs** directory named **NewEventDialog.cs** and add the following code.</span></span>

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

    <span data-ttu-id="56385-114">これは、ユーザーに予定表にイベントを追加するために必要な値を求めるダイアログを表示する新しいダイアログのシェルです。</span><span class="sxs-lookup"><span data-stu-id="56385-114">This is the shell of a new dialog that will prompt the user for the values needed to add an event to their calendar.</span></span>

1. <span data-ttu-id="56385-115">**NewEventDialog** クラスに次の関数を追加して、件名の入力をユーザーに求めます。</span><span class="sxs-lookup"><span data-stu-id="56385-115">Add the following function to the **NewEventDialog** class to prompt the user for a subject.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. <span data-ttu-id="56385-116">**NewEventDialog** クラスに次の関数を追加して、ユーザーが前の手順で指定した件名を格納し、出席者を追加するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="56385-116">Add the following function to the **NewEventDialog** class to store the subject the user gave in the previous step, and to ask if they want to add attendees.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. <span data-ttu-id="56385-117">**NewEventDialog** クラスに次の関数を追加して、前の手順でユーザーの応答を確認し、必要に応じてユーザーに出席者の一覧を要求します。</span><span class="sxs-lookup"><span data-stu-id="56385-117">Add the following function to the **NewEventDialog** class to check the user's response from the previous step and prompt the user for a list of attendees if needed.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. <span data-ttu-id="56385-118">**NewEventDialog** クラスに次の関数を追加して、前の手順で出席者のリストを保存します (存在する場合)。また、開始日時の入力を求めるメッセージをユーザーに表示します。</span><span class="sxs-lookup"><span data-stu-id="56385-118">Add the following function to the **NewEventDialog** class to store the list of attendees from the previous step (if present) and prompt the user for a start date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. <span data-ttu-id="56385-119">**NewEventDialog** クラスに次の関数を追加して、前の手順の開始値を格納し、終了日時を指定するようユーザーに求めるメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="56385-119">Add the following function to the **NewEventDialog** class to store the start value from the previous step and prompt the user for an end date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. <span data-ttu-id="56385-120">前の手順の最後の値を格納するために、次の関数を **NewEventDialog** クラスに追加し、ユーザーにすべての入力を確認するように求めます。</span><span class="sxs-lookup"><span data-stu-id="56385-120">Add the following function to the **NewEventDialog** class to store the end value from the previous step and ask the user to confirm all of the inputs.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. <span data-ttu-id="56385-121">前の手順でユーザーの応答を確認するには、 **NewEventDialog** クラスに次の関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="56385-121">Add the following function to the **NewEventDialog** class to check the user's response from the previous step.</span></span> <span data-ttu-id="56385-122">ユーザーが入力を確認した場合は、 **Oauthprompt** クラスを使用してアクセストークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="56385-122">If the user confirms the inputs, use the **OAuthPrompt** class to get an access token.</span></span> <span data-ttu-id="56385-123">それ以外の場合は、ダイアログを終了します。</span><span class="sxs-lookup"><span data-stu-id="56385-123">Otherwise, end the dialog.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. <span data-ttu-id="56385-124">**NewEventDialog** クラスに次の関数を追加して、MICROSOFT Graph SDK を使用して新しいイベントを作成します。</span><span class="sxs-lookup"><span data-stu-id="56385-124">Add the following function to the **NewEventDialog** class to use the Microsoft Graph SDK to create the new event.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    <span data-ttu-id="56385-125">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="56385-125">Consider what this code does.</span></span>

    - <span data-ttu-id="56385-126">ユーザーの優先タイムゾーンを決定するために、ユーザーの **MailboxSettings** を取得します。</span><span class="sxs-lookup"><span data-stu-id="56385-126">It gets the user's **MailboxSettings** to determine the user's preferred time zone.</span></span>
    - <span data-ttu-id="56385-127">ユーザーによって提供される値を使用して、 **Event** オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="56385-127">It creates an **Event** object using the values provided by the user.</span></span> <span data-ttu-id="56385-128">**Start** および **End** プロパティは、ユーザーのタイムゾーンで設定されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="56385-128">Note that the **Start** and **End** properties are set with the user's time zone.</span></span>
    - <span data-ttu-id="56385-129">ユーザーの予定表にイベントを作成します。</span><span class="sxs-lookup"><span data-stu-id="56385-129">It creates the event on the user's calendar.</span></span>

## <a name="add-validation"></a><span data-ttu-id="56385-130">検証を追加する</span><span class="sxs-lookup"><span data-stu-id="56385-130">Add validation</span></span>

<span data-ttu-id="56385-131">次に、Microsoft Graph でイベントを作成するときのエラーを回避するために、ユーザーの入力に検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="56385-131">Now add validation to the user's input to avoid errors when creating the event with Microsoft Graph.</span></span> <span data-ttu-id="56385-132">次のことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="56385-132">We want to make sure that:</span></span>

- <span data-ttu-id="56385-133">ユーザーが出席者リストを指定する場合は、有効な電子メールアドレスをセミコロンで区切ったリストにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="56385-133">If the user gives an attendee list, it should be a semicolon-delimited list of valid email addresses.</span></span>
- <span data-ttu-id="56385-134">開始日時は、有効な日付と時刻である必要があります。</span><span class="sxs-lookup"><span data-stu-id="56385-134">The start date/time should be a valid date and time.</span></span>
- <span data-ttu-id="56385-135">終了日時は、有効な日付と時刻である必要があり、開始日よりも後にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="56385-135">The end date/time should be a valid date and time, and should be later than the start.</span></span>

1. <span data-ttu-id="56385-136">**NewEventDialog** クラスに次の関数を追加して、ユーザーの出席者の入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="56385-136">Add the following function to the **NewEventDialog** class to validate the user's entry for attendees.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. <span data-ttu-id="56385-137">**NewEventDialog** クラスに次の関数を追加して、ユーザーの開始日時の入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="56385-137">Add the following functions to the **NewEventDialog** class to validate the user's entry for start date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. <span data-ttu-id="56385-138">**NewEventDialog** クラスに次の関数を追加して、ユーザーの終了日時の入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="56385-138">Add the following function to the **NewEventDialog** class to validate the user's entry for end date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a><span data-ttu-id="56385-139">WaterfallDialog に手順を追加する</span><span class="sxs-lookup"><span data-stu-id="56385-139">Add steps to WaterfallDialog</span></span>

<span data-ttu-id="56385-140">これで、ダイアログのすべての "手順" が完了したので、最後の手順として、それらをコンストラクターの **WaterfallDialog** に追加して、 **NewEventDialog** を **maindialog** に追加します。</span><span class="sxs-lookup"><span data-stu-id="56385-140">Now that you have all of the "steps" for the dialog, the last step is to add them to a **WaterfallDialog** in the constructor, then add the **NewEventDialog** to the **MainDialog**.</span></span>

1. <span data-ttu-id="56385-141">**NewEventDialog** コンストラクタを見つけて、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="56385-141">Locate the **NewEventDialog** constructor and add the following code to it.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    <span data-ttu-id="56385-142">これにより、使用されているすべてのダイアログが追加され、 **WaterfallDialog** の手順として実装したすべての関数が追加されます。</span><span class="sxs-lookup"><span data-stu-id="56385-142">This adds all of the dialogs that are used, and adds all of the functions you implemented as steps in the **WaterfallDialog**.</span></span>

1. <span data-ttu-id="56385-143">[] を開き、次の行をコンストラクターに追加し **ます。**</span><span class="sxs-lookup"><span data-stu-id="56385-143">Open **./Dialogs/MainDialog.cs** and add the following line to the constructor.</span></span>

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. <span data-ttu-id="56385-144">ブロック内のコードを `else if (command.StartsWith("add event"))` 次のように置き換え `ProcessStepAsync` ます。</span><span class="sxs-lookup"><span data-stu-id="56385-144">Replace the code inside the `else if (command.StartsWith("add event"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. <span data-ttu-id="56385-145">すべての変更を保存し、bot を再起動します。</span><span class="sxs-lookup"><span data-stu-id="56385-145">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="56385-146">Bot フレームワークエミュレーターを使用して bot に接続し、ログインします。</span><span class="sxs-lookup"><span data-stu-id="56385-146">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="56385-147">[ **Add event** ] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="56385-147">Select the **Add event** button.</span></span>

1. <span data-ttu-id="56385-148">プロンプトに応答して、新しいイベントを作成します。</span><span class="sxs-lookup"><span data-stu-id="56385-148">Respond to the prompts to create a new event.</span></span> <span data-ttu-id="56385-149">Start および end の値を入力するように求められた場合は、"today at 3PM" または "now" のような語句を使用することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="56385-149">Note that when prompted for start and end values, you can use phrases like "today at 3PM", or "now".</span></span>

    ![Bot フレームワークエミュレーターの [イベントの追加] ダイアログのスクリーンショット](images/add-event.png)

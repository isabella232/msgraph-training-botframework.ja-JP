---
ms.openlocfilehash: 05b3223967bf2a6d321c00cca079cc5c5b8ff0db
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347879"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fe6e5-101">この演習では、Bot に認証を実装するために Bot フレームワークの **Oauthprompt** を使用し、MICROSOFT Graph API を呼び出すためのアクセストークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-101">In this exercise you will use the Bot Framework's **OAuthPrompt** to implement authentication in the bot, and acquire access tokens for calling the Microsoft Graph API.</span></span>

1. <span data-ttu-id="fe6e5-102">**/appsettings.jsを** 開き、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-102">Open **./appsettings.json** and make the following changes.</span></span>

    - <span data-ttu-id="fe6e5-103">の値を、 `MicrosoftAppId` **グラフの予定表 Bot** アプリの登録のアプリケーション ID に変更します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-103">Change the value of `MicrosoftAppId` to the application ID of your **Graph Calendar Bot** app registration.</span></span>
    - <span data-ttu-id="fe6e5-104">の値 `MicrosoftAppPassword` を **Graph Calendar Bot** クライアントシークレットに変更します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-104">Change the value of `MicrosoftAppPassword` to your **Graph Calendar Bot** client secret.</span></span>
    - <span data-ttu-id="fe6e5-105">という値を `ConnectionName` 持つ値を追加 `GraphBotAuth` します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-105">Add a value named `ConnectionName` with a value of `GraphBotAuth`.</span></span>

    :::code language="json" source="../demo/GraphCalendarBot/appsettings.example.json":::

    > [!NOTE]
    > <span data-ttu-id="fe6e5-106">`GraphBotAuth`Azure Portal の **OAuth 接続設定** でエントリの名前以外の値を使用した場合は、その値をエントリに使用し `ConnectionName` ます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-106">If you used a value other than `GraphBotAuth` for the name of your entry in **OAuth Connection Settings** in the Azure Portal, use that value for the `ConnectionName` entry.</span></span>

## <a name="implement-dialogs"></a><span data-ttu-id="fe6e5-107">ダイアログを実装する</span><span class="sxs-lookup"><span data-stu-id="fe6e5-107">Implement dialogs</span></span>

1. <span data-ttu-id="fe6e5-108">**ダイアログ** という名前のプロジェクトのルートに新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-108">Create a new directory in the root of the project named **Dialogs**.</span></span> <span data-ttu-id="fe6e5-109">**LogoutDialog.cs** という名前のフォルダーに新しいファイルを作成し、次のコードを追加します **。**</span><span class="sxs-lookup"><span data-stu-id="fe6e5-109">Create a new file in the **./Dialogs** directory named **LogoutDialog.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/LogoutDialog.cs" id="LogoutDialogSnippet":::

    <span data-ttu-id="fe6e5-110">このダイアログは、派生元の bot の他のすべてのダイアログの基本クラスを提供します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-110">This dialog provides a base class for all of the other dialogs in the bot to derive from.</span></span> <span data-ttu-id="fe6e5-111">これにより、ユーザーは bot のダイアログ内の場所に関係なく、ログアウトすることができます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-111">This allows the user to log out no matter where they are in the bot's dialogs.</span></span>

1. <span data-ttu-id="fe6e5-112">**MainDialog.cs** という名前のフォルダーに新しいファイルを作成し、次のコードを追加します **。**</span><span class="sxs-lookup"><span data-stu-id="fe6e5-112">Create a new file in the **./Dialogs** directory named **MainDialog.cs** and add the following code.</span></span>

    ```csharp
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Dialogs.Choices;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;

    namespace CalendarBot.Dialogs
    {
        public class MainDialog : LogoutDialog
        {
            const string NO_PROMPT = "no-prompt";
            protected readonly ILogger _logger;

            public MainDialog(IConfiguration configuration, ILogger<MainDialog> logger)
                : base(nameof(MainDialog), configuration["ConnectionName"])
            {
                _logger = logger;

                // OAuthPrompt dialog handles the authentication and token
                // acquisition
                AddDialog(new OAuthPrompt(
                    nameof(OAuthPrompt),
                    new OAuthPromptSettings
                    {
                        ConnectionName = ConnectionName,
                        Text = "Please login",
                        Title = "Login",
                        Timeout = 300000, // User has 5 minutes to login
                    }));

                AddDialog(new ChoicePrompt(nameof(ChoicePrompt)));

                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    LoginPromptStepAsync,
                    ProcessLoginStepAsync,
                    PromptUserStepAsync,
                    CommandStepAsync,
                    ProcessStepAsync,
                    ReturnToPromptStepAsync
                }));

                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }

            private async Task<DialogTurnResult> LoginPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessLoginStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                // Get the token from the previous step. If it's there, login was successful
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;
                    if (!string.IsNullOrEmpty(tokenResponse?.Token))
                    {
                        await stepContext.Context.SendActivityAsync(
                            MessageFactory.Text("You are now logged in."), cancellationToken);
                        return await stepContext.NextAsync(null, cancellationToken);
                    }
                }

                await stepContext.Context.SendActivityAsync(
                    MessageFactory.Text("Login was not successful please try again."), cancellationToken);
                return await stepContext.EndDialogAsync();
            }

            private async Task<DialogTurnResult> PromptUserStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                var options = new PromptOptions
                {
                    Prompt = MessageFactory.Text("Please choose an option below"),
                    Choices = new List<Choice> {
                        new Choice { Value = "Show token" },
                        new Choice { Value = "Show me" },
                        new Choice { Value = "Show calendar" },
                        new Choice { Value = "Add event" },
                        new Choice { Value = "Log out" },
                    }
                };

                return await stepContext.PromptAsync(
                    nameof(ChoicePrompt),
                    options,
                    cancellationToken);
            }

            private async Task<DialogTurnResult> CommandStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Save the command the user entered so we can get it back after
                // the OAuthPrompt completes
                var foundChoice = stepContext.Result as FoundChoice;
                // Result could be a FoundChoice (if user selected a choice button)
                // or a string (if user just typed something)
                stepContext.Values["command"] = foundChoice?.Value ?? stepContext.Result;

                // There is no reason to store the token locally in the bot because we can always just call
                // the OAuth prompt to get the token or get a new token if needed. The prompt completes silently
                // if the user is already signed in.
                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;

                    // If we have the token use the user is authenticated so we may use it to make API calls.
                    if (tokenResponse?.Token != null)
                    {
                        var command = ((string)stepContext.Values["command"] ?? string.Empty).ToLowerInvariant();

                        if (command.StartsWith("show token"))
                        {
                            // Show the user's token - for testing and troubleshooting
                            // Generally production apps should not display access tokens
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text($"Your token is: {tokenResponse.Token}"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show me"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show calendar"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("add event"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I'm sorry, I didn't understand. Please try again."),
                                cancellationToken);
                        }
                    }
                }
                else
                {
                    await stepContext.Context.SendActivityAsync(
                        MessageFactory.Text("We couldn't log you in. Please try again later."),
                        cancellationToken);
                    return await stepContext.EndDialogAsync(cancellationToken: cancellationToken);
                }

                // Go to the next step
                return await stepContext.NextAsync(cancellationToken: cancellationToken);
            }

            private async Task<DialogTurnResult> ReturnToPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Restart the dialog, but skip the initial login prompt
                return await stepContext.ReplaceDialogAsync(InitialDialogId, NO_PROMPT, cancellationToken);
            }
        }
    }
    ```

    <span data-ttu-id="fe6e5-113">このコードを確認してみてください。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-113">Take a moment to review this code.</span></span>

    - <span data-ttu-id="fe6e5-114">コンストラクターでは、順序どおりに実行される一連の手順を含む [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) を設定します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-114">In the constructor, it sets up a [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) with a set of steps that occur in order.</span></span>
        - <span data-ttu-id="fe6e5-115">で `LoginPromptStepAsync` は、 **Oauthprompt** を送信します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-115">In `LoginPromptStepAsync` it sends an **OAuthPrompt**.</span></span> <span data-ttu-id="fe6e5-116">ユーザーがログインしていない場合は、UI プロンプトがユーザーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-116">If the user isn't logged in, this will send a UI prompt to the user.</span></span>
        - <span data-ttu-id="fe6e5-117">では `ProcessLoginStepAsync` 、ログインが成功したかどうかを確認し、確認を送信します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-117">In `ProcessLoginStepAsync` it checks if the login was successful and sends a confirmation.</span></span>
        - <span data-ttu-id="fe6e5-118">ここでは、 `PromptUserStepAsync` 使用可能なコマンドを使用して **ChoicePrompt** を送信します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-118">In `PromptUserStepAsync` it sends a **ChoicePrompt** with the available commands.</span></span>
        - <span data-ttu-id="fe6e5-119">これにより `CommandStepAsync` 、ユーザーの選択が保存され、 **Oauthprompt** が再送信されます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-119">In `CommandStepAsync` it saves the user's choice, then resends an **OAuthPrompt**.</span></span>
        - <span data-ttu-id="fe6e5-120">では `ProcessStepAsync` 、受け取ったコマンドに基づいてアクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-120">In `ProcessStepAsync` it takes action based on the command received.</span></span>
        - <span data-ttu-id="fe6e5-121">では、 `ReturnToPromptStepAsync` ウォーターフォールが開始されますが、最初のユーザーログインをスキップするフラグが渡されます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-121">In `ReturnToPromptStepAsync` it starts the waterfall over, but passes a flag to skip the initial user login.</span></span>

## <a name="update-calendarbot"></a><span data-ttu-id="fe6e5-122">CalendarBot を更新する</span><span class="sxs-lookup"><span data-stu-id="fe6e5-122">Update CalendarBot</span></span>

<span data-ttu-id="fe6e5-123">次の手順では、これらの新しいダイアログを使用するように **Calendarbot** を更新します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-123">The next step is to update **CalendarBot** to use these new dialogs.</span></span>

1. <span data-ttu-id="fe6e5-124">/Bots/CalendarBot.cs を開き、内容全体を次のコードで置き換えます **。**</span><span class="sxs-lookup"><span data-stu-id="fe6e5-124">Open **./Bots/CalendarBot.cs** and replace its entire contents with the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Bots/CalendarBot.cs" id="CalendarBotSnippet":::

    <span data-ttu-id="fe6e5-125">変更の概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-125">Here's a brief summary of the changes.</span></span>

    - <span data-ttu-id="fe6e5-126">**Calendarbot** クラスがテンプレートクラスに変更され、**ダイアログ** が受け取ります。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-126">Changed the **CalendarBot** class to be a template class, receiving a **Dialog**.</span></span>
    - <span data-ttu-id="fe6e5-127">**Teamsactivityhandler** を拡張するように **calendarbot** クラスを変更し、Microsoft Teams でのサインインを許可しました。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-127">Changed the **CalendarBot** class to extend **TeamsActivityHandler**, allowing it to sign in in Microsoft Teams.</span></span>
    - <span data-ttu-id="fe6e5-128">認証を有効にするためのメソッドオーバーライドを追加しました。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-128">Added additional method overrides to enable authentication.</span></span>

## <a name="update-startupcs"></a><span data-ttu-id="fe6e5-129">Startup.cs の更新</span><span class="sxs-lookup"><span data-stu-id="fe6e5-129">Update Startup.cs</span></span>

<span data-ttu-id="fe6e5-130">最後の手順として、 `ConfigureServices` 認証に必要なサービスを追加するメソッドを更新し、[新規作成] ダイアログボックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-130">The final step is to update the `ConfigureServices` method to add the services needed for authentication and the new dialog.</span></span>

1. <span data-ttu-id="fe6e5-131">**Startup.cs** を開き、 `services.AddTransient<IBot, Bots.CalendarBot>();` メソッドから行を削除します。 `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="fe6e5-131">Open **./Startup.cs** and remove the `services.AddTransient<IBot, Bots.CalendarBot>();` line from the `ConfigureServices` method.</span></span>

1. <span data-ttu-id="fe6e5-132">メソッドの最後に、次のコードを挿入 `ConfigureServices` します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-132">Insert the following code at the end of the `ConfigureServices` method.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="ConfigureServiceSnippet":::

## <a name="test-authentication"></a><span data-ttu-id="fe6e5-133">認証のテスト</span><span class="sxs-lookup"><span data-stu-id="fe6e5-133">Test authentication</span></span>

1. <span data-ttu-id="fe6e5-134">すべての変更を保存し、を使用して bot を開始し `dotnet run` ます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-134">Save all of your changes and start the bot with `dotnet run`.</span></span>

1. <span data-ttu-id="fe6e5-135">Bot フレームワークエミュレーターを開きます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-135">Open the Bot Framework Emulator.</span></span> <span data-ttu-id="fe6e5-136">[ **ファイル** ] メニューの [ **新しい Bot 構成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-136">Select the **File** menu, then **New Bot Configuration...**.</span></span>

1. <span data-ttu-id="fe6e5-137">フィールドに次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-137">Fill in the fields as follows.</span></span>

    - <span data-ttu-id="fe6e5-138">**Bot 名:**`CalendarBot`</span><span class="sxs-lookup"><span data-stu-id="fe6e5-138">**Bot name:** `CalendarBot`</span></span>
    - <span data-ttu-id="fe6e5-139">**エンドポイント URL:**`https://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="fe6e5-139">**Endpoint URL:** `https://localhost:3978/api/messages`</span></span>
    - <span data-ttu-id="fe6e5-140">**Microsoft アプリ id:** **Graph Calendar Bot** アプリの登録のアプリケーション id</span><span class="sxs-lookup"><span data-stu-id="fe6e5-140">**Microsoft App ID:** the application ID of your **Graph Calendar Bot** app registration</span></span>
    - <span data-ttu-id="fe6e5-141">**Microsoft アプリのパスワード:** 使用している **グラフの予定表ボット** クライアントのシークレット</span><span class="sxs-lookup"><span data-stu-id="fe6e5-141">**Microsoft App password:** your **Graph Calendar Bot** client secret</span></span>
    - <span data-ttu-id="fe6e5-142">**Bot 構成に格納されているキーを暗号化します。** い</span><span class="sxs-lookup"><span data-stu-id="fe6e5-142">**Encrypt keys stored in your bot configuration:** Enabled</span></span>

    ![[新しいボット構成] ダイアログのスクリーンショット](images/new-bot-config.png)

1. <span data-ttu-id="fe6e5-144">[ **保存して接続**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-144">Select **Save and connect**.</span></span> <span data-ttu-id="fe6e5-145">エミュレーターを接続すると、 `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`</span><span class="sxs-lookup"><span data-stu-id="fe6e5-145">After the emulator connects, you should see `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`</span></span>

1. <span data-ttu-id="fe6e5-146">何らかのテキストを入力して bot に送信します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-146">Type some text and send it to the bot.</span></span> <span data-ttu-id="fe6e5-147">Bot は、ログインプロンプトで応答します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-147">The bot responds with a login prompt.</span></span>

1. <span data-ttu-id="fe6e5-148">[ **ログイン** ] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-148">Select the **Login** button.</span></span> <span data-ttu-id="fe6e5-149">エミュレーターは、で始まる URL の確認を求めるメッセージを表示し `oauthlink://https://token.botframeworkcom` ます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-149">The emulator prompts you to confirm the URL that starts with `oauthlink://https://token.botframeworkcom`.</span></span> <span data-ttu-id="fe6e5-150">[ **確認** ] を選択して続行します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-150">Select **Confirm** to continue.</span></span>

1. <span data-ttu-id="fe6e5-151">ポップアップウィンドウで、Microsoft 365 アカウントを使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-151">In the pop-up window, login with your Microsoft 365 account.</span></span> <span data-ttu-id="fe6e5-152">要求されたアクセス許可を確認し、同意します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-152">Review the requested permissions and accept.</span></span>

1. <span data-ttu-id="fe6e5-153">認証と同意が完了すると、ポップアップウィンドウに検証コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-153">Once authentication and consent are complete, the pop-up window provides a validation code.</span></span> <span data-ttu-id="fe6e5-154">コードをコピーし、ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-154">Copy the code and close the window.</span></span>

    ![Bot フレームワークエミュレーター検証コードのスクリーンショット](images/validation-code.png)

1. <span data-ttu-id="fe6e5-156">[チャット] ウィンドウに検証コードを入力して、ログインを完了します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-156">Enter the validation code in the chat window to complete the login.</span></span>

    ![サンプル bot とのログイン会話のスクリーンショット](images/bot-login.png)

1. <span data-ttu-id="fe6e5-158">[ **トークンの表示** ] ボタン (または種類) を選択すると、 `show token` bot にアクセストークンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-158">If you select the **Show token** button (or type `show token`), the bot displays the access token.</span></span> <span data-ttu-id="fe6e5-159">[ **ログアウト] ボタン** (または入力) によって、ログアウト `log out` できます。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-159">The **Log out** button (or typing `log out`) will log you out.</span></span>

> [!TIP]
> <span data-ttu-id="fe6e5-160">Bot との会話を開始するときに、Bot フレームワークエミュレーターに次のエラーメッセージが表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-160">You may receive the following error message in the Bot Framework Emulator when starting a conversation with the bot.</span></span>
>
> ```text
> Failed to generate an actual sign-in link: Error: Failed to connect to ngrok instance for OAuth postback URL:
> FetchError: request to http://127.0.0.1:4041/api/tunnels failed, reason: connect ECONNREFUSED 127.0.0.1:4041
> ```
>
> <span data-ttu-id="fe6e5-161">この問題が発生した場合は、エミュレーターを閉じて再起動します。</span><span class="sxs-lookup"><span data-stu-id="fe6e5-161">If this happens, close the emulator and restart it.</span></span>

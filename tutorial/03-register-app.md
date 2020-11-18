---
ms.openlocfilehash: 9a320225c7e7e76506d73909a311fa019b1f74f3
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347858"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ced72-101">この演習では、Azure ポータルを使用して、新しい Bot チャネル登録と Azure AD web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="ced72-101">In this exercise, you will create a new Bot Channels registration and an Azure AD web application registration using the Azure Portal.</span></span>

## <a name="create-a-bot-channels-registration"></a><span data-ttu-id="ced72-102">Bot チャネル登録を作成する</span><span class="sxs-lookup"><span data-stu-id="ced72-102">Create a Bot Channels registration</span></span>

1. <span data-ttu-id="ced72-103">ブラウザーを開き、 [Azure ポータル](https://portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="ced72-103">Open a browser and navigate to the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="ced72-104">Azure サブスクリプションに関連付けられているアカウントを使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="ced72-104">Login using the account associated with your Azure subscription.</span></span>

1. <span data-ttu-id="ced72-105">左上のメニューを選択し、[ **リソースの作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-105">Select the upper-left menu, then select **Create a resource**.</span></span>

    ![Azure Portal メニューのスクリーンショット](images/create-resource.png)

1. <span data-ttu-id="ced72-107">[ **新規** ] ページで、[ `Bot Channel` **Bot チャネル登録**] を検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-107">On the **New** page, search for `Bot Channel` and select **Bot Channels Registration**.</span></span>

1. <span data-ttu-id="ced72-108">[ **Bot チャネル登録** ] ページで、[ **作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-108">On the **Bot Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="ced72-109">必要なフィールドに入力し、[ **メッセージングエンドポイント** を空白のままにします。]</span><span class="sxs-lookup"><span data-stu-id="ced72-109">Fill in the required fields, and leave **Messaging endpoint** blank.</span></span> <span data-ttu-id="ced72-110">**Bot ハンドル** フィールドは一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="ced72-110">The **Bot handle** field must be unique.</span></span> <span data-ttu-id="ced72-111">さまざまな価格レベルを確認し、シナリオに適したものを選択してください。</span><span class="sxs-lookup"><span data-stu-id="ced72-111">Be sure to review the different pricing tiers and select what makes sense for your scenario.</span></span> <span data-ttu-id="ced72-112">これが学習課題である場合は、無料のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-112">If this is just a learning exercise, you may want to select the free option.</span></span>

1. <span data-ttu-id="ced72-113">**Microsoft アプリ ID とパスワード** を選択し、[**新規作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-113">Select the **Microsoft App ID and password**, then select **Create New**.</span></span>

1. <span data-ttu-id="ced72-114">[ **アプリ登録ポータルでアプリ ID を作成** する] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-114">Select **Create App ID in the App Registration Portal**.</span></span> <span data-ttu-id="ced72-115">これにより、Azure Portal の **アプリ登録** ブレードに新しいウィンドウまたはタブが開きます。</span><span class="sxs-lookup"><span data-stu-id="ced72-115">This will open a new window or tab to the **App registrations** blade in the Azure Portal.</span></span>

1. <span data-ttu-id="ced72-116">[ **アプリの登録** ] ブレードで、[ **新規登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-116">In the **App registrations** blade, select **New registration**.</span></span>

1. <span data-ttu-id="ced72-117">値を次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-117">Set the values as follows.</span></span>

    - <span data-ttu-id="ced72-118">`Graph Calendar Bot` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-118">Set **Name** to `Graph Calendar Bot`.</span></span>
    - <span data-ttu-id="ced72-119">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ced72-120">**[リダイレクト URI]** を空のままにします。</span><span class="sxs-lookup"><span data-stu-id="ced72-120">Leave **Redirect URI** empty.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](./images/aad-register-an-app.png)

1. <span data-ttu-id="ced72-122">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-122">Select **Register**.</span></span> <span data-ttu-id="ced72-123">[ **Graph Calendar Bot** ] ページで、 **アプリケーション (クライアント) ID** の値をコピーして保存します。これは、次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="ced72-123">On the **Graph Calendar Bot** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](./images/aad-application-id.png)

1. <span data-ttu-id="ced72-125">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-125">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="ced72-126">
            \*\*[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-126">Select the **New client secret** button.</span></span> <span data-ttu-id="ced72-127">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-127">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="ced72-128">このページを離れる前に、クライアント シークレットの値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="ced72-128">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="ced72-129">次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="ced72-129">You will need it in the following steps.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ced72-130">このクライアント シークレットは今後表示されないため、この段階で必ずコピーするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="ced72-130">This client secret is never shown again, so make sure you copy it now.</span></span> <span data-ttu-id="ced72-131">この値は、安全に保つために複数の場所に入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ced72-131">You will need to enter this value in multiple places so keep it safe.</span></span>

1. <span data-ttu-id="ced72-132">ブラウザーの [Bot チャネルの登録] ウィンドウに戻り、アプリケーション ID を **Microsoft APP id** フィールドに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="ced72-132">Return to the Bot Channel Registration window in your browser, and paste the application ID into the **Microsoft App ID** field.</span></span> <span data-ttu-id="ced72-133">クライアントシークレットを [ **パスワード** ] フィールドに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="ced72-133">Paste your client secret into the **Password** field.</span></span> <span data-ttu-id="ced72-134">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-134">Select **OK**.</span></span>

1. <span data-ttu-id="ced72-135">[ **Bot チャネル登録** ] ページで、[ **作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-135">On the **Bots Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="ced72-136">Bot チャネル登録が作成されるのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="ced72-136">Wait for the Bot Channels registration to be created.</span></span> <span data-ttu-id="ced72-137">作成されたら、Azure ポータルのホームページに戻り、[ **Bot サービス**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-137">Once created, return to the Home page in the Azure Portal, then select **Bot Services**.</span></span> <span data-ttu-id="ced72-138">新しい Bot チャネル登録を選択して、そのプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="ced72-138">Select your new Bots Channel registration to view its properties.</span></span>

## <a name="create-a-web-app-registration"></a><span data-ttu-id="ced72-139">Web アプリの登録を作成する</span><span class="sxs-lookup"><span data-stu-id="ced72-139">Create a web app registration</span></span>

1. <span data-ttu-id="ced72-140">Azure ポータルの [ **アプリの登録** ] セクションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="ced72-140">Return to the **App registrations** section of the Azure Portal.</span></span>

1. <span data-ttu-id="ced72-141">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-141">Select **New registration**.</span></span> <span data-ttu-id="ced72-142">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-142">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="ced72-143">`Graph Calendar Bot Auth` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-143">Set **Name** to `Graph Calendar Bot Auth`.</span></span>
    - <span data-ttu-id="ced72-144">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-144">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ced72-145">**[リダイレクト URI]** で、最初のドロップダウン リストを `Web` に設定し、それから `https://token.botframework.com/.auth/web/redirect` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="ced72-145">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://token.botframework.com/.auth/web/redirect`.</span></span>

1. <span data-ttu-id="ced72-146">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-146">Select **Register**.</span></span> <span data-ttu-id="ced72-147">[ **グラフ予定表 Bot 認証** ] ページで、 **アプリケーション (クライアント) ID** の値をコピーして保存します。これは、次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="ced72-147">On the **Graph Calendar Bot Auth** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

1. <span data-ttu-id="ced72-148">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-148">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="ced72-149">
            \*\*[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-149">Select the **New client secret** button.</span></span> <span data-ttu-id="ced72-150">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-150">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="ced72-151">このページを離れる前に、クライアント シークレットの値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="ced72-151">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="ced72-152">次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="ced72-152">You will need it in the following steps.</span></span>

1. <span data-ttu-id="ced72-153">[ **API アクセス許可**] を選択し、[ **アクセス許可の追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-153">Select **API permissions**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="ced72-154">[ **Microsoft Graph**] を選択し、[委任された **アクセス許可**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-154">Select **Microsoft Graph**, then select **Delegated permissions**.</span></span>

1. <span data-ttu-id="ced72-155">次のアクセス許可を選択し、[ **アクセス許可の追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-155">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="ced72-156">**openid**</span><span class="sxs-lookup"><span data-stu-id="ced72-156">**openid**</span></span>
    - <span data-ttu-id="ced72-157">**profile**</span><span class="sxs-lookup"><span data-stu-id="ced72-157">**profile**</span></span>
    - <span data-ttu-id="ced72-158">**Calendars.ReadWrite**</span><span class="sxs-lookup"><span data-stu-id="ced72-158">**Calendars.ReadWrite**</span></span>
    - <span data-ttu-id="ced72-159">**MailboxSettings.Read**</span><span class="sxs-lookup"><span data-stu-id="ced72-159">**MailboxSettings.Read**</span></span>

    ![構成済みのアクセス許可のスクリーンショット](images/configured-permissions.png)

### <a name="about-permissions"></a><span data-ttu-id="ced72-161">アクセス許可について</span><span class="sxs-lookup"><span data-stu-id="ced72-161">About permissions</span></span>

<span data-ttu-id="ced72-162">これらのアクセス許可スコープによって、bot が実行できること、およびそのユーザーがどのように使用するかを検討します。</span><span class="sxs-lookup"><span data-stu-id="ced72-162">Consider what each of those permission scopes allows the bot to do, and what the bot will use them for.</span></span>

- <span data-ttu-id="ced72-163">**openid** and **profile**: bot がユーザーにサインインして、Id トークン内の Azure AD からの基本情報を取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ced72-163">**openid** and **profile**: allows the bot to sign users in and get basic information from Azure AD in the identity token.</span></span>
- <span data-ttu-id="ced72-164">[**カレンダー**]: bot は、ユーザーの予定表を読み取って、ユーザーの予定表に新しいイベントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="ced72-164">**Calendars.ReadWrite**: allows the bot to read the user's calendar and to add new events to the user's calendar.</span></span>
- <span data-ttu-id="ced72-165">**MailboxSettings**: bot がユーザーのメールボックス設定を読み取ることができるようにします。</span><span class="sxs-lookup"><span data-stu-id="ced72-165">**MailboxSettings.Read**: allows the bot to read the user's mailbox settings.</span></span> <span data-ttu-id="ced72-166">Bot はこれを使用して、ユーザーが選択したタイムゾーンを取得します。</span><span class="sxs-lookup"><span data-stu-id="ced72-166">The bot will use this to get the user's selected time zone.</span></span>
- <span data-ttu-id="ced72-167">**ユーザーの読み取り**: Bot が Microsoft Graph からユーザーのプロファイルを取得することを許可します。</span><span class="sxs-lookup"><span data-stu-id="ced72-167">**User.Read**: allows the bot to get the user's profile from Microsoft Graph.</span></span> <span data-ttu-id="ced72-168">Bot はこれを使用して、ユーザーの名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="ced72-168">The bot will use this to get the user's name.</span></span>

## <a name="add-oauth-connection-to-the-bot"></a><span data-ttu-id="ced72-169">Bot に OAuth 接続を追加する</span><span class="sxs-lookup"><span data-stu-id="ced72-169">Add OAuth connection to the bot</span></span>

1. <span data-ttu-id="ced72-170">Azure Portal で bot の Bot チャネル登録ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="ced72-170">Navigate to your bot's Bot Channels Registration page on the Azure Portal.</span></span> <span data-ttu-id="ced72-171">[ **Bot 管理**] の下にある [**設定**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-171">Select **Settings** under **Bot Management**.</span></span>

1. <span data-ttu-id="ced72-172">ページの下部付近にある [ **OAuth 接続設定** ] で、[ **設定の追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-172">Under **OAuth Connection Settings** near the bottom of the page, select **Add Setting**.</span></span>

1. <span data-ttu-id="ced72-173">フォームに次のように入力し、[ **保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-173">Fill in the form as follows, then select **Save**.</span></span>

    - <span data-ttu-id="ced72-174">**Name**: `GraphBotAuth`</span><span class="sxs-lookup"><span data-stu-id="ced72-174">**Name**: `GraphBotAuth`</span></span>
    - <span data-ttu-id="ced72-175">**プロバイダー**: **Azure Active Directory v2**</span><span class="sxs-lookup"><span data-stu-id="ced72-175">**Provider**: **Azure Active Directory v2**</span></span>
    - <span data-ttu-id="ced72-176">**クライアント id**: **グラフ予定表 Bot 認証** 登録のアプリケーション id。</span><span class="sxs-lookup"><span data-stu-id="ced72-176">**Client id**: The application ID of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="ced72-177">**クライアントシークレット**: **グラフの予定表 Bot 認証** 登録のクライアントシークレット。</span><span class="sxs-lookup"><span data-stu-id="ced72-177">**Client secret**: The client secret of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="ced72-178">**トークン交換 URL**: 空白のままにする</span><span class="sxs-lookup"><span data-stu-id="ced72-178">**Token Exchange URL**: Leave blank</span></span>
    - <span data-ttu-id="ced72-179">**テナント ID**: `common`</span><span class="sxs-lookup"><span data-stu-id="ced72-179">**Tenant ID**: `common`</span></span>
    - <span data-ttu-id="ced72-180">**範囲**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span><span class="sxs-lookup"><span data-stu-id="ced72-180">**Scopes**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span></span>

1. <span data-ttu-id="ced72-181">[ **OAuth 接続設定**] の下の **GraphBotAuth** エントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-181">Select the **GraphBotAuth** entry under **OAuth Connection Settings**.</span></span>

1. <span data-ttu-id="ced72-182">[ **テスト接続**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-182">Select **Test Connection**.</span></span> <span data-ttu-id="ced72-183">これにより、新しいブラウザーウィンドウまたはタブが開き、OAuth フローが開始されます。</span><span class="sxs-lookup"><span data-stu-id="ced72-183">This opens a new browser window or tab to start the OAuth flow.</span></span>

1. <span data-ttu-id="ced72-184">必要に応じて、サインインします。</span><span class="sxs-lookup"><span data-stu-id="ced72-184">If necessary, sign in.</span></span> <span data-ttu-id="ced72-185">要求されたアクセス許可の一覧を確認し、[ **同意** する] を選択します。</span><span class="sxs-lookup"><span data-stu-id="ced72-185">Review the list of requested permissions, then select **Accept**.</span></span>

1. <span data-ttu-id="ced72-186">**' GraphBotAuth ' のテスト接続が成功** したことを確認するメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ced72-186">You should see a **Test Connection to 'GraphBotAuth' Succeeded** message.</span></span>

> [!TIP]
> <span data-ttu-id="ced72-187">このページの [ **トークンのコピー** ] ボタンを選択し、トークンをに貼り付けて、 [https://jwt.ms](https://jwt.ms) トークン内のクレームを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ced72-187">You can select the **Copy Token** button on this page, and paste the token into [https://jwt.ms](https://jwt.ms) to see the claims inside the token.</span></span> <span data-ttu-id="ced72-188">これは、認証エラーのトラブルシューティングに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="ced72-188">This is useful when troubleshooting authentication errors.</span></span>

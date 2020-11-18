---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347918"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b9c5e-101">このセクションでは、Microsoft Graph SDK を使用して、ログインしているユーザーを取得します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-101">In this section you'll use the Microsoft Graph SDK to get the logged-in user.</span></span>

## <a name="create-a-graph-service"></a><span data-ttu-id="b9c5e-102">Graph サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="b9c5e-102">Create a Graph service</span></span>

<span data-ttu-id="b9c5e-103">最初に、ボットが Microsoft Graph SDK から **Graphserviceclient** 取得するために使用できるサービスを実装してから、依存関係の挿入を通じてそのサービスを bot が使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-103">Start by implementing a service that the bot can use to get a **GraphServiceClient** from the Microsoft Graph SDK, then making that service available to the bot via dependency injection.</span></span>

1. <span data-ttu-id="b9c5e-104">プロジェクトのルートに **Graph** という名前の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-104">Create a new directory in the root of the project named **Graph**.</span></span> <span data-ttu-id="b9c5e-105">**IGraphClientService.cs** という名前のディレクトリに新しいファイルを作成し、次のコードを追加します **。**</span><span class="sxs-lookup"><span data-stu-id="b9c5e-105">Create a new file in the **./Graph** directory named **IGraphClientService.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. <span data-ttu-id="b9c5e-106">**GraphClientService.cs** という名前のディレクトリに新しいファイルを作成し、次のコードを追加します **。**</span><span class="sxs-lookup"><span data-stu-id="b9c5e-106">Create a new file in the **./Graph** directory named **GraphClientService.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. <span data-ttu-id="b9c5e-107">Startup.cs を開き、関数の最後に次のコードを追加します **。** `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="b9c5e-107">Open **./Startup.cs** and add the following code to the end of the `ConfigureServices` function.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. <span data-ttu-id="b9c5e-108">開いている名前を表示 **します。**</span><span class="sxs-lookup"><span data-stu-id="b9c5e-108">Open **./Dialogs/MainDialog.cs**.</span></span> <span data-ttu-id="b9c5e-109">次の `using` ステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-109">Add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. <span data-ttu-id="b9c5e-110">次のプロパティを **Maindialog** クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-110">Add the following property to the **MainDialog** class.</span></span>

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. <span data-ttu-id="b9c5e-111">**Maindialog** クラスのコンストラクターを見つけ、そのシグネチャを更新して、 **igraphserviceclient** すべてのパラメーターを取得します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-111">Locate the constructor for the **MainDialog** class and update its signature to take an **IGraphServiceClient** parameter.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. <span data-ttu-id="b9c5e-112">次のコードをコンストラクターに追加します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-112">Add the following code to the constructor.</span></span>

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a><span data-ttu-id="b9c5e-113">ログオンしているユーザーを取得する</span><span class="sxs-lookup"><span data-stu-id="b9c5e-113">Get the logged on user</span></span>

<span data-ttu-id="b9c5e-114">このセクションでは、Microsoft Graph を使用して、ユーザーの名前、電子メールアドレス、および写真を取得します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-114">In this section you'll use the Microsoft Graph to get the user's name, email address, and photo.</span></span> <span data-ttu-id="b9c5e-115">次に、アダプティブカードを作成して情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-115">Then you'll create an Adaptive Card to show the information.</span></span>

1. <span data-ttu-id="b9c5e-116">**CardHelper.cs** という名前のプロジェクトのルートに新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-116">Create a new file in the root of the project named **CardHelper.cs**.</span></span> <span data-ttu-id="b9c5e-117">次のコードをファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-117">Add the following code to the file.</span></span>

    ```csharp
    using AdaptiveCards;
    using Microsoft.Graph;
    using System;
    using System.IO;

    namespace CalendarBot
    {
        public class CardHelper
        {
            public static AdaptiveCard GetUserCard(User user, Stream photo)
            {
              // Create an Adaptive Card to display the user
                // See https://adaptivecards.io/designer/ for possibilities
                var userCard = new AdaptiveCard("1.2");

                var columns = new AdaptiveColumnSet();
                userCard.Body.Add(columns);

                var userPhotoColumn = new AdaptiveColumn { Width = AdaptiveColumnWidth.Auto };
                columns.Columns.Add(userPhotoColumn);

                userPhotoColumn.Items.Add(new AdaptiveImage {
                    Style = AdaptiveImageStyle.Person,
                    Size = AdaptiveImageSize.Small,
                    Url = GetDataUriFromPhoto(photo)
                });

                var userInfoColumn = new AdaptiveColumn {Width = AdaptiveColumnWidth.Stretch };
                columns.Columns.Add(userInfoColumn);

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Weight = AdaptiveTextWeight.Bolder,
                    Wrap = true,
                    Text = user.DisplayName
                });

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Spacing = AdaptiveSpacing.None,
                    IsSubtle = true,
                    Wrap = true,
                    Text = user.Mail ?? user.UserPrincipalName
                });

                return userCard;
            }

            private static Uri GetDataUriFromPhoto(Stream photo)
            {
                // Copy to a MemoryStream to get access to bytes
                var photoStream = new MemoryStream();
                photo.CopyTo(photoStream);

                var photoBytes = photoStream.ToArray();

                return new Uri($"data:image/png;base64,{Convert.ToBase64String(photoBytes)}");
            }
        }
    }
    ```

    <span data-ttu-id="b9c5e-118">このコードでは、 **AdaptiveCard** NuGet パッケージを使用して、ユーザーを表示するアダプティブカードを構築します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-118">This code uses the **AdaptiveCard** NuGet package to build an Adaptive Card to display the user.</span></span>

1. <span data-ttu-id="b9c5e-119">次の関数を **Maindialog** クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-119">Add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    <span data-ttu-id="b9c5e-120">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-120">Consider what this code does.</span></span>

    - <span data-ttu-id="b9c5e-121">**Graphclient** を使用して [、ログイン](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0)しているユーザーを取得します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-121">It uses the **graphClient** to [get the logged-in user](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0).</span></span>
        - <span data-ttu-id="b9c5e-122">メソッドを使用して、 `Select` 返されるフィールドを制限します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-122">It uses the `Select` method to limit which fields are returned.</span></span>
    - <span data-ttu-id="b9c5e-123">**Graphclient** を使用して [ユーザーの写真を取得](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0)し、サポートされている最小サイズの48x48 ピクセルを要求します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-123">It uses the **graphClient** to [get the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0), requesting the smallest supported size of 48x48 pixels.</span></span>
    - <span data-ttu-id="b9c5e-124">**Cardhelper** クラスを使用して、アダプティブカードを作成し、カードを添付ファイルとして送信します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-124">It uses the **CardHelper** class to construct an Adaptive Card and sends the card as an attachment.</span></span>

1. <span data-ttu-id="b9c5e-125">ブロック内のコードを `else if (command.StartsWith("show me"))` 次のように置き換え `ProcessStepAsync` ます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-125">Replace the code inside the `else if (command.StartsWith("show me"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. <span data-ttu-id="b9c5e-126">すべての変更を保存し、bot を再起動します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-126">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="b9c5e-127">Bot フレームワークエミュレーターを使用して bot に接続し、ログインします。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-127">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="b9c5e-128">[ **自分を表示** する] ボタンを選択して、ログオンしているユーザーを表示します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-128">Select the **Show me** button to display the logged-on user.</span></span>

    ![ユーザーが表示されているアダプティブカードのスクリーンショット](images/user-card.png)

---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347918"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、Microsoft Graph SDK を使用して、ログインしているユーザーを取得します。

## <a name="create-a-graph-service"></a>Graph サービスを作成する

最初に、ボットが Microsoft Graph SDK から **Graphserviceclient** 取得するために使用できるサービスを実装してから、依存関係の挿入を通じてそのサービスを bot が使用できるようにします。

1. プロジェクトのルートに **Graph** という名前の新しいディレクトリを作成します。 **IGraphClientService.cs** という名前のディレクトリに新しいファイルを作成し、次のコードを追加します **。**

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. **GraphClientService.cs** という名前のディレクトリに新しいファイルを作成し、次のコードを追加します **。**

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. Startup.cs を開き、関数の最後に次のコードを追加します **。** `ConfigureServices`

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. 開いている名前を表示 **します。** 次の `using` ステートメントをファイルの先頭に追加します。

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. 次のプロパティを **Maindialog** クラスに追加します。

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. **Maindialog** クラスのコンストラクターを見つけ、そのシグネチャを更新して、 **igraphserviceclient** すべてのパラメーターを取得します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. 次のコードをコンストラクターに追加します。

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a>ログオンしているユーザーを取得する

このセクションでは、Microsoft Graph を使用して、ユーザーの名前、電子メールアドレス、および写真を取得します。 次に、アダプティブカードを作成して情報を表示します。

1. **CardHelper.cs** という名前のプロジェクトのルートに新しいファイルを作成します。 次のコードをファイルに追加します。

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

    このコードでは、 **AdaptiveCard** NuGet パッケージを使用して、ユーザーを表示するアダプティブカードを構築します。

1. 次の関数を **Maindialog** クラスに追加します。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    このコードの内容を検討してください。

    - **Graphclient** を使用して [、ログイン](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0)しているユーザーを取得します。
        - メソッドを使用して、 `Select` 返されるフィールドを制限します。
    - **Graphclient** を使用して [ユーザーの写真を取得](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0)し、サポートされている最小サイズの48x48 ピクセルを要求します。
    - **Cardhelper** クラスを使用して、アダプティブカードを作成し、カードを添付ファイルとして送信します。

1. ブロック内のコードを `else if (command.StartsWith("show me"))` 次のように置き換え `ProcessStepAsync` ます。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. すべての変更を保存し、bot を再起動します。

1. Bot フレームワークエミュレーターを使用して bot に接続し、ログインします。 [ **自分を表示** する] ボタンを選択して、ログオンしているユーザーを表示します。

    ![ユーザーが表示されているアダプティブカードのスクリーンショット](images/user-card.png)

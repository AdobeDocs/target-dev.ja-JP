---
title: .NET SDK のインストール
description: をインストールする方法を説明します。 [!DNL Adobe Target] .NET SDK.
feature: APIs/SDKs
exl-id: 3cc84775-4692-4d14-9e82-db2873140835
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '60'
ht-degree: 0%

---

# .NET SDK のインストール

.NET SDK の配布元： [NuGet](https://www.nuget.org/packages/Adobe.Target.Client). 開始するには、経由でをインストールして、依存関係として追加します。 `Package Manage` または `.NET CLI`:

## パッケージマネージャー

>[!BEGINTABS]

>[!TAB パッケージマネージャー]

```csharp {line-numbers="true"}
Install-Package Adobe.Target.Client
```

>[!TAB .NET CLI]

```csharp {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!ENDTABS]

オープンソースコードは、次の場所にあります。 [https://github.com/adobe/target-dotnet-sdk](https://github.com/adobe/target-dotnet-sdk).

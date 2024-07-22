---
title: .NET SDK のインストール
description: ' [!DNL Adobe Target] .NET SDK のインストール方法を説明します。'
feature: APIs/SDKs
exl-id: 3cc84775-4692-4d14-9e82-db2873140835
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# .NET SDK のインストール

.NET SDK は、[NuGet](https://www.nuget.org/packages/Adobe.Target.Client) によって配布されます。 開始するには、`Package Manage` または `.NET CLI` を使用してをインストールし、依存関係として追加します。

## パッケージマネージャー

>[!BEGINTABS]

>[!TAB  パッケージマネージャー ]

```csharp {line-numbers="true"}
Install-Package Adobe.Target.Client
```

>[!TAB .NET CLI]

```csharp {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!ENDTABS]

オープンソースのコードは、[https://github.com/adobe/target-dotnet-sdk](https://github.com/adobe/target-dotnet-sdk) にあります。

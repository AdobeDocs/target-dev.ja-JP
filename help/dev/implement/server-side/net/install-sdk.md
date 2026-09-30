---
title: .NET SDKのインストール
description: '[!DNL Adobe Target] .NET SDKのインストール方法について説明します。'
feature: APIs/SDKs
exl-id: 3cc84775-4692-4d14-9e82-db2873140835
TQID: 'https://experienceleague.adobe.com/438ax3dEUclYIa42EvOLyBBuRngsZLHRDXabumxp0F8'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: a19e8738-9679-599a-b83b-5f2f15f8e4d6
    internal-label: APIs/SDKs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 9%
---
# .NET SDKのインストール

.NET SDKは[NuGet](https://www.nuget.org/packages/Adobe.Target.Client)によって配布されています。 開始するには、`Package Manage`または`.NET CLI`経由でインストールして、依存関係として追加します。

## パッケージマネージャー

>[!BEGINTABS]

>[!TAB  パッケージマネージャー]

```csharp {line-numbers="true"}
Install-Package Adobe.Target.Client
```

>[!TAB .NET CLI]

```csharp {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!ENDTABS]

オープン ソースのコードは、[https://github.com/adobe/target-dotnet-sdk](https://github.com/adobe/target-dotnet-sdk)にあります。

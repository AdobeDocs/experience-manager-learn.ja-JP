---
title: ファイルシステムへのペイロードドキュメントの書き込み
description: ペイロードフォルダー下にある書き込みドキュメントをファイルシステムに追加するカスタムプロセスステップです。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '140'
ht-degree: 100%

---

# ファイルシステムへのドキュメントの書き込み

一般的なユース ケースは、ワークフローで生成されたドキュメントをファイル システムに書き込むことです。
このカスタムワークフロープロセスの手順により、ワークフロードキュメントをファイルシステムに簡単に書き込むことができます。
カスタムプロセスでは、次のコンマ区切りの引数を使用します。

```java
ChangeBeneficiary.pdf,c:\confirmation
```

最初の引数は、ファイルシステムに保存するドキュメントの名前です。2 つ目の引数は、ドキュメントを保存するフォルダーの場所です。例えば、上記のユースケースでは、ドキュメントは `c:\confirmation\ChangeBeneficiary.pdf` に書き込まれます。

次のスクリーンショットは、カスタムプロセスの手順に渡す必要のある引数を示しています
![write-payload-file-system](assets/write-payload-file-system.png)

[カスタムバンドルは、ここからダウンロードできます。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)

---
title: ペイロードドキュメントをファイルシステムに書き込む
description: ペイロードフォルダーの下にある書き込みドキュメントをファイルシステムに追加するカスタムプロセスステップ
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# ドキュメントをファイルシステムに書き込む

一般的な使用例は、ワークフローで生成されたドキュメントをファイルシステムに書き込む場合です。
このカスタムワークフロープロセスステップでは、ワークフロードキュメントをファイルシステムに簡単に書き込むことができます。
カスタムプロセスでは、次のコンマ区切りの引数を使用します。

```java
ChangeBeneficiary.pdf,c:\confirmation
```

最初の引数は、ファイルシステムに保存するドキュメントの名前です。 2 つ目の引数は、ドキュメントを保存するフォルダの場所です。 例えば、上記の使用例では、ドキュメントはc:\confirmation\ChangeBeneficiary.pdfに書き込まれます

次のスクリーンショットに、カスタムプロセスステップに渡す必要のある引数を示します
![write-payload-file-system](assets/write-payload-file-system.png)

[カスタムバンドルは、ここからダウンロードできます。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
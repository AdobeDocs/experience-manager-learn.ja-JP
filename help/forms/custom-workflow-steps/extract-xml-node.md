---
title: ペイロードドキュメントをファイルシステムに書き込む
description: ペイロードフォルダーの下にある書き込みドキュメントをファイルシステムに追加するカスタムプロセスステップ
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# 送信されたデータ xml からノードを抽出

このカスタムプロセスの手順では、別の xml ドキュメントからノードを抽出して新しい xml ドキュメントを作成します。 送信したデータを xdp テンプレートと結合して pdf を生成する場合は、この機能を使用する必要があります。 例えば、アダプティブフォームを送信する際、xdp テンプレートと統合する必要があるデータは、データ要素内にあります。 この場合、適切なデータ要素を抽出して、別の xml ドキュメントを作成する必要があります。

次のスクリーンショットに、カスタムプロセスステップに渡す必要のある引数を示します
![process-step](assets/create-xml-process-step.png)
次にパラメーターを示します
* Data.xml — ノードを抽出する xml ファイル
* datatomerge.xml — 抽出したノードで作成された新しい xml
* /afData/afUnboundData/data — 抽出するノード


次のスクリーンショットは、ペイロードフォルダーの下に作成された datamerge.xml を示しています
![create-xml](assets/create-xml.png)

[カスタムバンドルは、ここからダウンロードできます。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
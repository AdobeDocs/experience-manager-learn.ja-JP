---
title: 送信されたデータ xml からノードを抽出
description: ペイロードフォルダーの下にある書き込みドキュメントをファイルシステムに追加するカスタムプロセスステップ
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '179'
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

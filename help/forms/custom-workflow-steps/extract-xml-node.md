---
title: 送信されたデータ xml からのノードの抽出
description: ペイロードフォルダー下にある書き込みドキュメントをファイルシステムに追加するカスタムプロセスステップです。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '179'
ht-degree: 100%

---

# 送信されたデータ xml からのノードの抽出

このカスタムプロセスステップでは、別の xml ドキュメントからノードを抽出して新しい xml ドキュメントを作成します。 送信したデータを xdp テンプレートと結合して PDF を生成する場合は、この機能を使用する必要があります。 例えば、アダプティブフォームを送信する際、xdp テンプレートと統合する必要があるデータは、データ要素内にあります。 この場合、適切なデータ要素を抽出して、別の xml ドキュメントを作成する必要があります。

カスタムプロセスステップに渡す必要のある引数を次のスクリーンショットに示します。
![process-step](assets/create-xml-process-step.png)
以下にパラメーターを示します。
* Data.xml - ノードを抽出する xml ファイル
* datatomerge.xml - 抽出したノードで作成された新しい xml
* /afData/afUnboundData/data - 抽出するノード


次のスクリーンショットは、ペイロードフォルダー下に作成される datamerge.xml を示しています。
![create-xml](assets/create-xml.png)

[カスタムバンドルは、ここからダウンロードできます。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)

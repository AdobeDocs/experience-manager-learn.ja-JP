---
title: AEM Formsでのトランザクションレポートの使用
description: AEM Forms のトランザクションレポートを使用すると、AEM Forms のデプロイメントで指定した日以降に発生したすべてのトランザクション数を保持できます。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 29%

---

# AEM Formsでのトランザクションレポートの使用{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1 では、フォーム送信数、ドキュメントサービスを使用したドキュメントのレンダリング、インタラクティブ通信（Web チャネルと印刷チャネル）のレンダリングを行うトランザクションレポートが導入されました。この機能は、主に、フォーム送信数やドキュメント数に基づいてソフトウェアのライセンスを取得します。 この機能は、現在、AEM Forms OSGi スタックでのみ使用できます。

## 取引レポートの有効化 {#enabling-transaction-reporting}

デフォルトでは、トランザクションの記録は無効になっています。 トランザクションの記録を有効にするには、次の手順に従ってください。

* [configMgr を開きます。](http://localhost:4502/system/console/configMgr)
* 「Forms Transaction Reporting」を検索します。
* 「トランザクションの記録」チェックボックスを選択します。
* 変更を保存します

トランザクションレポートが有効になったら、アダプティブFormsを送信し、ドキュメントサービスを使用してドキュメントを生成するか、またはインタラクティブ通信のドキュメントをレンダリングして、トランザクションレポートの動作を確認できます。

## 取引レポートの表示 {#viewing-transaction-report}

トランザクションレポートを表示するには、管理者としてAEM Formsにログインします。 fd-Administrator グループのメンバーのみがトランザクションレポートを表示できます。

ツールを選択 | Forms |トランザクションレポートの表示

または、 [ここ](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransactionReporting](assets/transactionreporting.gif)

上のスクリーンショットの「処理済みのドキュメント」は、ドキュメントサービスを使用して生成されたドキュメントの数です。 レンダリングされたドキュメントは、レンダリングされたインタラクティブ通信ドキュメント（Web および Print）の数です。 Forms Submitted は、アダプティブフォーム送信の数です。

トランザクションは、指定した期間（フラッシュバッファ時間 + リバースレプリケーション時間）、バッファに残ります。デフォルトでは、トランザクション数がトランザクションレポートに反映されるまで、約 90 秒かかります。

PDF フォームの送信、エージェント UI によるインタラクティブ通信のプレビュー、非標準のフォーム送信方法の使用などのアクションは、トランザクションとしては考慮されません。AEM Forms は、このようなトランザクションを記録する API を提供します。カスタム実装から API を呼び出して、トランザクションを記録します。

オーサーインスタンスでトランザクションレポートを表示している場合は、すべてのパブリッシュインスタンスでリバースレプリケーションが設定されていることを確認します。

トランザクションレポートの詳細を確認するには [ここをクリックしてください](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

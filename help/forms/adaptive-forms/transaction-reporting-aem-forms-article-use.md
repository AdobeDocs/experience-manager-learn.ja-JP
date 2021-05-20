---
title: AEM Formsでのトランザクションレポートの使用
seo-title: AEM Formsでのトランザクションレポートの使用
description: AEM Formsのトランザクションレポートを使用すると、AEM Formsデプロイメントで指定した日以降に実行されたすべてのトランザクションの数を保持できます。
seo-description: AEM Formsのトランザクションレポートを使用すると、AEM Formsデプロイメントで指定した日以降に実行されたすべてのトランザクションの数を保持できます。
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: アダプティブフォーム
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 1%

---


# AEM Forms{#using-transaction-reporting-in-aem-forms}でのトランザクションレポートの使用

AEM Forms 6.4.1では、フォーム送信数、ドキュメントサービスを使用したドキュメントのレンダリング、インタラクティブ通信（Webチャネルと印刷チャネル）のレンダリングを取得するトランザクションレポートが導入されました。この機能は、主にフォーム送信数やドキュメント数に基づいてソフトウェアのライセンスを取得します。 この機能は、現在、AEM Forms OSGiスタックでのみ使用できます。

## トランザクションレポートの有効化{#enabling-transaction-reporting}

デフォルトでは、トランザクションの記録は無効です。 トランザクションの記録を有効にするには、次の手順に従ってください。

* [configMgrを開きます。](http://localhost:4502/system/console/configMgr)
* 「Forms Transaction Reporting」を検索します。
* 「トランザクションの記録」チェックボックスを選択します。
* 変更を保存します

トランザクションレポートが有効になると、アダプティブFormsを送信したり、ドキュメントサービスを使用してドキュメントを生成したり、インタラクティブ通信のドキュメントをレンダリングしてトランザクションレポートの動作を確認したりできます。

## トランザクションレポートの表示{#viewing-transaction-report}

トランザクションレポートを表示するには、管理者としてAEM Formsにログインします。 fd-Administratorグループのメンバーのみがトランザクションレポートを表示できます。

ツールを選択 | Forms |トランザクションレポートの表示

または、[ここ](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)をクリックしてトランザクションレポートを表示します。

![TransactionReporting](assets/transactionreporting.gif)

上のスクリーンショットの「処理済みのドキュメント」は、ドキュメントサービスを使用して生成されたドキュメントの数です。 レンダリングされたドキュメントは、レンダリングされたインタラクティブ通信ドキュメント（WebおよびPrint）の数です。 Forms Submittedは、アダプティブフォーム送信の数です。

トランザクションは、指定された期間（フラッシュバッファ時間+リバースレプリケーション時間）、バッファに残ります。 デフォルトでは、トランザクション数がトランザクションレポートに反映されるまでに約90秒かかります。

PDFフォームの送信、エージェントUIを使用したインタラクティブ通信のプレビュー、非標準のフォーム送信方法の使用などのアクションは、トランザクションとしては考慮されません。 AEM Formsは、このようなトランザクションを記録するAPIを提供します。 カスタム実装からAPIを呼び出して、トランザクションを記録します。

オーサーインスタンスでトランザクションレポートを表示する場合は、すべてのパブリッシュインスタンスでリバースレプリケーションが設定されていることを確認します。

トランザクションレポート[の詳細については、](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)をクリックしてください。


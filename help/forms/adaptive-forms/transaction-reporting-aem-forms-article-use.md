---
title: AEM Formsでの取引レポートの使用
seo-title: AEM Formsでの取引レポートの使用
description: AEM Formsのトランザクションレポートを使用すると、AEM Forms導入の指定日以降に行われたすべてのトランザクションをカウントできます。
seo-description: AEM Formsのトランザクションレポートを使用すると、AEM Forms導入の指定日以降に行われたすべてのトランザクションをカウントできます。
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: アダプティブフォーム
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 2%

---


# AEM Formsでのトランザクションレポートの使用{#using-transaction-reporting-in-aem-forms}

AEM Forms6.4.1では、フォームの送信数、ドキュメントサービスを使用したドキュメントのレンダリング、対話型通信(Webおよび印刷チャネル)のレンダリングを取得するトランザクションレポートが導入されています。この機能は、主にフォームの送信数やドキュメント数に基づいてソフトウェアのライセンスを取得するお客様向けです。 この機能は現在、AEM FormsOSGIスタックでのみ使用できます。

## トランザクションレポートを有効にする{#enabling-transaction-reporting}

デフォルトでは、トランザクションの記録は無効になっています。 トランザクションの記録を有効にするには、次の手順に従います。

* [configMgrを開きます。](http://localhost:4502/system/console/configMgr)
* 「Forms取引レポート」の検索
* 「トランザクションの記録」チェックボックスを選択します
* 変更を保存する

トランザクションレポートを有効にすると、アダプティブFormsを送信し、ドキュメントサービスを使用してドキュメントを生成するか、Interactive Communicationドキュメントをレンダリングして、実行中のトランザクションレポートを確認できます。

## トランザクションレポートの表示{#viewing-transaction-report}

トランザクションレポートを表示するには、管理者としてAEM Formsにログインします。 fd-Administratorグループのメンバーのみがトランザクションレポートを表示できます。

ツールの選択 |Forms |表示トランザクションレポート

または、[ここ](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)をクリックしてトランザクションレポートを表示します。

![TransactionReporting](assets/transactionreporting.gif)

上のスクリーンショットでは、「処理済みのドキュメント」は、ドキュメントサービスを使用して生成されたドキュメントの数です。 レンダリングされるドキュメントは、レンダリングされるInteractive Communicationドキュメント（WebおよびPrint）の数です。 「Forms送信済み」は、アダプティブフォームの送信数です。

トランザクションは、指定された期間（フラッシュ・バッファ時間+逆複製時間）、バッファに残ります。 デフォルトでは、トランザクション数がトランザクションレポートに反映されるまでに約90秒かかります。

PDFフォームの送信、エージェントUIを使用した対話型通信のプレビュー、または非標準のフォーム送信方法を使用したアクションは、トランザクションとして考慮されません。 AEM Formsは、このようなトランザクションを記録するAPIを提供しています。 カスタム実装からAPIを呼び出して、トランザクションを記録します。

作成者インスタンスに関するトランザクションレポートを表示している場合は、すべての発行インスタンスで逆複製が設定されていることを確認します。

トランザクションレポート[の詳細については、こちら](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)をクリックしてください


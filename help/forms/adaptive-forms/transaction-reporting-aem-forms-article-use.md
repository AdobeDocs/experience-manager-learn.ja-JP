---
title: AEM Forms でのトランザクションレポートの使用
description: AEM Forms のトランザクションレポートを使用すると、AEM Forms のデプロイメントで指定した日以降に発生したすべてのトランザクション数を保持できます。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 100%

---

# AEM Forms でのトランザクションレポートの使用{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1 では、フォーム送信数、ドキュメントサービスを使用したドキュメントのレンダリング、およびインタラクティブ通信（web チャネルと印刷チャネル）のレンダリングを取り込むトランザクションレポートが導入されました。この機能は、AEM Forms OSGi スタック用の AEM Forms 6.4.1 および AEM Forms JEE スタック用の 6.5.20 で導入されています。

## トランザクションレポートの有効化 {#enabling-transaction-reporting}

デフォルトでは、トランザクションの記録は無効になっています。 トランザクションの記録を有効にするには、次の手順に従ってください。

* [configMgr を開く](http://localhost:4502/system/console/configMgr)
* 「Forms トランザクションレポート」を検索します。
* 「トランザクションの記録」チェックボックスを選択します。
* 変更内容を保存します。

トランザクションレポートが有効になったら、アダプティブフォームを送信し、ドキュメントサービスを使用してドキュメントを生成するか、またはインタラクティブ通信のドキュメントをレンダリングして、トランザクションレポートの動作を確認できます。

## トランザクションレポートの表示 {#viewing-transaction-report}

トランザクションレポートを表示するには、管理者として AEM Forms にログインします。 トランザクションレポートを表示できるのは、fd-Administrator グループのメンバーだけです。

ツール／Forms／トランザクションレポートの表示を選択します

または、[こちら](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)をクリックしてトランザクションレポートを表示します

![TransactionReporting](assets/transactionreporting.gif)

上のスクリーンショットの「処理済みのドキュメント」は、ドキュメントサービスを使用して生成されたドキュメントの数です。 「レンダリングされたドキュメント」は、レンダリングされたインタラクティブ通信ドキュメント（web および印刷）の数です。 「送信されたフォーム」は、アダプティブフォームの送信数です。

トランザクションは、指定した期間（フラッシュバッファ時間 + リバースレプリケーション時間）、バッファに残ります。 デフォルトでは、トランザクション数がトランザクションレポートに反映されるまで、約 90 秒かかります。

PDF フォームの送信、エージェント UI によるインタラクティブ通信のプレビュー、非標準のフォーム送信方法の使用などのアクションは、トランザクションとしては考慮されません。 AEM Forms は、このようなトランザクションを記録する API を提供します。 カスタム実装から API を呼び出して、トランザクションを記録します。

オーサーインスタンスでトランザクションレポートを表示している場合は、すべてのパブリッシュインスタンスでリバースレプリケーションが設定されていることを確認します。

トランザクションレポートについて詳しくは、[こちら](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/transaction-reports-overview.html)をクリックしてください

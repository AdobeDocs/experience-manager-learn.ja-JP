---
title: AEM UI 拡張機能のデプロイ
description: AEM UI 拡張機能のデプロイ方法を説明します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
duration: 166
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '765'
ht-degree: 100%

---

# 拡張機能のデプロイ

AEM as a Cloud Service 環境で使用するには、拡張機能の Application Builder アプリをデプロイして承認する必要があります。

拡張機能の Application Builder アプリをデプロイする際に注意すべき考慮事項がいくつかあります。

+ 拡張機能は、Adobe Developer Console プロジェクトワークスペースにデプロイされます。 デフォルトのワークスペースを以下に示します。
   + __実稼動__&#x200B;ワークスペースには、すべての AEM as a Cloud Service で使用できる拡張機能のデプロイメントが含まれています。
   + __ステージング__ワークスペースは、開発者ワークスペースとして機能します。ステージングワークスペースにデプロイされた拡張機能は、AEM as a Cloud Service では使用できません。
Adobe Developer Console ワークスペースには、AEM as a Cloud Service 環境タイプとの直接的な関連はありません。
+ 実稼動ワークスペースにデプロイされた拡張機能は、拡張機能が存在するアドビ組織内のすべての AEM as a Cloud Service 環境に表示されます。
拡張機能は、[AEM as a Cloud Service ホスト名を確認する条件付きロジック](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments)を追加することで、登録されている環境に限定することはできません。
+ AEM as a Cloud Service では、複数の拡張機能を使用できます。アドビでは、それぞれの Application Builder 拡張機能アプリを使用して 1 つのビジネス目標を解決することをお勧めします。ただし、1 つの Application Builder 拡張機能アプリで、共通のビジネス目標をサポートする複数の拡張ポイントを実装できます。

## 初期デプロイメント

拡張機能を AEM as a Cloud Service 環境で使用できるようにするには、Adobe Developer Console にデプロイする必要があります。

デプロイメントプロセスは、次の 2 つの論理的なステップに分かれています。

1. 開発者による Adobe Developer Console への Application Builder 拡張機能アプリのデプロイメント。
1. デプロイメントマネージャーまたはビジネスオーナーによる拡張機能の承認。

### 拡張機能のデプロイ

拡張機能を実稼動ワークスペースにデプロイします。実稼動ワークスペースにデプロイされた拡張機能は、拡張機能をデプロイする先のアドビ組織にあるすべての AEM as a Cloud Service オーサーサービスに自動的に追加されます。

1. 更新された拡張機能 App Builder アプリのルートへのコマンドラインを開きます。
1. 実稼動ワークスペースがアクティブであることを確認します。

   ```shell
   $ aio app use -w Production
   ```

   `.env` と `.aio` への変更を結合します。

1. 更新された拡張機能の App Builder アプリをデプロイします。

   ```shell
   $ aio app deploy
   ```

#### デプロイメントの承認をリクエスト

![承認用に拡張機能を送信](./assets/deploy/submit-for-approval.png){align="center"}

1. [Adobe Developer Console](https://developer.adobe.com) にログインします
1. __コンソール__&#x200B;を選択します
1. __プロジェクト__&#x200B;に移動します
1. 拡張機能に関連付けられたプロジェクトを選択します
1. __実稼動__&#x200B;ワークスペースを選択します
1. __承認用に送信__&#x200B;を選択します
1. 必要に応じてフィールドを更新し、フォームに入力して送信します

### デプロイメントの承認

![拡張機能を承認](./assets/deploy/adobe-exchange.png){align="center"}

1. [Adobe Exchange](https://exchange.adobe.com/) にログインします
1. __管理__／__レビュー保留中のアプリ__&#x200B;に移動します
1. 拡張機能の App Builder アプリを&#x200B;__レビュー__&#x200B;します
1. 拡張機能の変更を許可できる場合、レビューを&#x200B;__承認__&#x200B;します。これにより、アドビ組織内のすべての AEM as a Cloud Service オーサーサービスに拡張機能が直ちに挿入されます。

拡張機能のリクエストが承認されると、AEM as a Cloud Service のオーサーサービスで拡張機能が直ちにアクティブになります。

## 拡張機能の更新

App Builder 拡張機能アプリの更新は、[初期デプロイメント](#initial-deployment)と同じプロセスに従いますが、既存の拡張機能のデプロイメントを最初に失効させる必要がある点が異なります。

### 拡張機能の失効

新しいバージョンの拡張機能をデプロイするには、まずその拡張機能を失効させる（削除する）必要があります。拡張機能は、失効している間、AEM コンソールでは使用できません。

1. [Adobe Exchange](https://exchange.adobe.com/) にログインします。
1. __管理__／__App Builder アプリ__&#x200B;に移動します。
1. 更新する拡張機能を&#x200B;__失効__&#x200B;させます。

### 拡張機能のデプロイ

拡張機能を実稼動ワークスペースにデプロイします。実稼動ワークスペースにデプロイされた拡張機能は、拡張機能をデプロイする先のアドビ組織にあるすべての AEM as a Cloud Service オーサーサービスに自動的に追加されます。

1. 更新された拡張機能 App Builder アプリのルートへのコマンドラインを開きます。
1. 実稼動ワークスペースがアクティブであることを確認します。

   ```shell
   $ aio app use -w Production
   ```

   `.env` と `.aio` への変更を結合します。

1. 更新された拡張機能の App Builder アプリをデプロイします。

   ```shell
   $ aio app deploy
   ```

#### デプロイメントの承認をリクエスト

![承認用に拡張機能を送信](./assets/deploy/submit-for-approval.png){align="center"}

1. [Adobe Developer Console](https://developer.adobe.com) にログインします
1. __コンソール__&#x200B;を選択します
1. __プロジェクト__&#x200B;に移動します
1. 拡張機能に関連付けられたプロジェクトを選択します
1. __実稼動__&#x200B;ワークスペースを選択します
1. __承認用に送信__&#x200B;を選択します
1. 必要に応じてフィールドを更新し、フォームに入力して送信します

#### デプロイメントのリクエストを承認

![拡張機能を承認](./assets/deploy/adobe-exchange.png){align="center"}

1. [Adobe Exchange](https://exchange.adobe.com/) にログインします
1. __管理__／__レビュー保留中のアプリ__&#x200B;に移動します
1. 拡張機能の App Builder アプリを&#x200B;__レビュー__&#x200B;します
1. 拡張機能の変更を許可できる場合、レビューを&#x200B;__承認__&#x200B;します。これにより、アドビ組織内のすべての AEM as a Cloud Service オーサーサービスに拡張機能が直ちに挿入されます。

拡張機能のリクエストが承認されると、AEM as a Cloud Service のオーサーサービスで拡張機能が直ちにアクティブになります。

## 拡張機能の削除

![拡張機能を削除](./assets/deploy/revoke.png)

拡張機能を削除するには、Adobe Exchange から失効させる（削除する）必要があります。拡張機能が失効されると、すべての AEM as a Cloud Service オーサーサービスから削除されます。

1. [Adobe Exchange](https://exchange.adobe.com/) にログインします
1. __管理__／__App Builder アプリ__&#x200B;に移動します
1. 削除する拡張機能を&#x200B;__失効__&#x200B;させます

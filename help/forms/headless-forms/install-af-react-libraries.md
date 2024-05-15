---
title: 必要なアダプティブフォームの React ライブラリのインストール
description: React プロジェクトへの必要な依存関係の追加
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 100%

---

# 必要な依存関係のインストール

React プロジェクトでヘッドレスアダプティブフォームの使用を開始するには、React プロジェクトに次の依存関係をインストールします

* @aemforms/af-react-components
* @aemforms/af-react-renderer

package.json を更新して、次の依存関係を含めます。執筆時点では 0.22.41 が現在のバージョンでした

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>このチュートリアルのドロップダウンリストとカードレイアウトは、[マテリアル UI ライブラリ](https://mui.com/)を使用して作成しました。コードをシステムで動作させるには、適切なマテリアル UI パッケージをダウンロードする必要があります。

## プロキシの設定

クロスオリジンリソース共有（CORS）は、アプリがホストされているドメインとは異なるドメインに web ブラウザーからリクエストを送信することを制限するセキュリティメカニズムです。別のドメインでホストされている API からデータを取得しようとすると、CORS エラーが発生することがあります。プロキシを設定すると、CORS の制限を回避して、React アプリから API にリクエストを送信できます。src フォルダー内の setUpProxy.js というファイルで、次のコードを使用しています。**パブリッシュインスタンスを指すようにターゲットを変更する必要があります。**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

また、**http-proxy-middleware** モジュールをインストールしてプロジェクトに追加する必要もあります。

## 次の手順

[埋め込むフォームの取得](./fetch-the-form.md)

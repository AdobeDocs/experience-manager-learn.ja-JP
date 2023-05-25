---
title: 必要なアダプティブフォームの React ライブラリのインストール
description: React プロジェクトに必要な依存関係を追加する
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---


# 必要な依存関係のインストール

React プロジェクトでヘッドレスアダプティブフォームの使用を開始するには、React プロジェクトに次の依存関係をインストールします

* @aemforms/af-react-components
* @aemforms/af-react-renderer

package.json を更新して、次の依存関係を含めます。 書き込み時0.22.41は現在のバージョンでした

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>このチュートリアルのドロップダウンリストとカードレイアウトは、 [マテリアル UI ライブラリ](https://mui.com/). 適切なマテリアル UI パッケージをダウンロードして、コードをシステムで動作させる必要があります。

## プロキシを設定

クロスオリジンリソース共有 (CORS) は、アプリがホストされているドメインとは異なるドメインに Web ブラウザーからリクエストを送信することを制限するセキュリティメカニズムです。 別のドメインでホストされている API からデータを取得しようとすると、CORS エラーが発生する場合があります。 プロキシを設定すると、CORS の制限を回避して、React アプリから API にリクエストを送信できます。 src フォルダー内の setUpProxy.js というファイルで、次のコードを使用しています。 **ターゲットを、パブリッシュインスタンスを指すように変更してください。**

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

また、 **http-proxy-middleware** モジュールをプロジェクトに追加します。

## 次の手順

[埋め込むフォームを取得](./fetch-the-form.md)
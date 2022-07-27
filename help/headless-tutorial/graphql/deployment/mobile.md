---
title: AEMヘッドレスモバイルデプロイメント
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# AEMヘッドレスモバイルデプロイメント

AEMヘッドレスモバイルデプロイメントは、iOS、Android などのネイティブモバイルアプリです。 コンテンツをヘッドレスな方法で消費し、AEMとやり取りする

AEMヘッドレス API への HTTP 接続はブラウザーのコンテキストでは開始されないので、モバイルデプロイメントには最小限の設定が必要です。

## デプロイメント設定

| モバイルアプリの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [CORS 設定](./cors.md) | ✘ | ✘ | ✘ |
| 画像 URL のホスト名 | ✔ | ✔ | ✔ |

## モバイルアプリの例

Adobeには、例えば、iOSおよび Android モバイルアプリが用意されています。



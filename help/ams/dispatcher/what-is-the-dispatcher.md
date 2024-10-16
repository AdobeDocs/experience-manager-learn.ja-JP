---
title: 「Dispatcher」とは
description: Dispatcher の実像を理解します。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '256'
ht-degree: 100%

---

# 「Dispatcher」とは

[目次](./overview.md)

AEM Dispatcher を必要とするものの基本的な説明から始めます。

## Apache web サーバー

Linux サーバーへの基本的な Apache web サーバーインストールから始めます。

Apache サーバーの動作に関する基本的な説明：

- 簡単なルールに従って、サーバーの静的ドキュメントディレクトリ（`DocumentRoot`）から HTTP(S) プロトコルを介してファイルを提供します
- デフォルトの場所（`/var/www/html`）に保存されたファイルがリクエストに対して照合され、リクエスト元のクライアントのブラウザーでレンダリングされます




## AEM 固有のモジュールファイル（`mod_dispatcher.so`）

次に、Dispatcher モジュールと呼ばれるプラグインを Apache web サーバーに追加します。

Adobe AEM Dispatcher モジュールの動作に関する基本的な説明：

- デフォルトのファイルハンドラーを強化する
- 不正なリクエストを除外／AEM のソフトベリーやエンドポイントを保護する
- 複数のレンダラーが存在する場合は、負荷分散を行う
- ライブキャッシュディレクトリを使用可能にする／停滞したファイルのフラッシュをサポートする
- すべての AMS インストールの前面に位置し、web サイトやアセットをクライアントのブラウザーに配信する
- リクエストをキャッシュして、AEM サーバーが単独で達成できるよりもはるかに高速にリクエストに応える
- その他多数...

## Web トラフィックワークフロー

基本的な Dispatcher サーバーを構築するために一緒にインストールされるコンポーネントがわかると、Adobe Manager Services 設定の基本的な web トラフィックワークフローを理解できます。
これは、AEM コンテンツの訪問者にコンテンツを提供するシステムのチェーンで Dispatcher が果たす役割を理解するのに役立ちます。

<b>既にキャッシュされているコンテンツの提供</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>AEM からの新しいコンテンツの提供</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>コンテンツの公開／変更</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[次は、基本的なファイルレイアウト](./basic-file-layout.md)

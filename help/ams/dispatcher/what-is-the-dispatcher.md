---
title: 「Dispatcher」とは
description: Dispatcher が実際に何であるかを理解します。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 5%

---

# 「Dispatcher」とは

[目次](./overview.md)

AEM Dispatcher の基本的な説明から始めます。

## Apache Web サーバー

Linux サーバーでの基本的な Apache Web サーバーインストールから始めます。

Apache サーバーの動作に関する基本的な説明です。

- 単純なルールに従って、静的ドキュメントディレクトリ (`DocumentRoot`)
- デフォルトの場所 (`/var/www/html`) は、リクエストに対して照合され、リクエスト元のクライアントのブラウザーでレンダリングされます。




## AEM固有のモジュールファイル (`mod_dispatcher.so`)

次に、Dispatcher モジュールと呼ばれるプラグインを Apache Web サーバーに追加します。

AEM Dispatcher モジュールの機能に関する基本的なAdobe:

- デフォルトのファイルハンドラーを強化します
- 不正なリクエストを除外/ AEMのソフトベリー/エンドポイントを保護
- 複数のレンダラーが存在する場合は、負荷分散が行われます。
- ライブキャッシュディレクトリを可能にする/停滞したファイルのフラッシュをサポート
- すべての AMS インストールの前面にあり、Web サイトやアセットをクライアントのブラウザーに配信します
- AEMサーバーが独自に達成できるよりもはるかに高速に再提供するリクエストをキャッシュします。
- その他…

## Web トラフィックワークフロー

基本的な Dispatcher サーバーを構築するために、どのコンポーネントが一緒にインストールされているかを理解すると、Adobeマネージャーサービス設定の基本的な Web トラフィックワークフローを理解できます。
これは、AEMコンテンツの訪問者にコンテンツを提供するシステムチェーン内で、AEM が果たす役割を理解するのに役立ちます。

<b>既にキャッシュされたコンテンツを提供中</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>AEMからの新しいコンテンツの提供</b>

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

<b>コンテンツの公開/変更</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[次へ —> 基本的なファイルレイアウト](./basic-file-layout.md)

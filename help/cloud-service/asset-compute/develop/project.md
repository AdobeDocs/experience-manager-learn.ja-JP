---
title: 「アセット計算」の拡張機能用の「アセット計算」プロジェクトの作成
description: アセット計算プロジェクトは、AdobeI/O CLIを使用して生成されるNode.jsプロジェクトで、特定の構造に従って、Adobe I/O Runtimeに展開し、AEMとCloud Serviceして統合できます。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# アセット計算プロジェクトの作成

アセット計算プロジェクトは、AdobeI/O CLIを使用して生成されるNode.jsプロジェクトで、特定の構造に従って、Adobe I/O Runtimeに展開し、AEMとCloud Serviceして統合できます。 単一のアセット計算プロジェクトには、1人または複数のアセット計算ワーカーを含めることができます。各ワーカーは、Cloud Service処理プロファイルとしてAEMから参照可能な、個別のHTTPエンドポイントを持ちます。

## プロジェクトの生成

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_アセット計算プロジェクトの生成時のクリックスルー（オーディオなし）_


I/O [AdobeI/O CLI Asset Computeプラグインを使用して](../set-up/development-environment.md#aio-cli) 、新しい空のAsset Computeプロジェクトを生成します。

1. コマンドラインから、プロジェクトを含むフォルダに移動します。
1. コマンドラインでを実行し、対話型プロジェクト生成CLI `aio app init` を開始します。
   + これにより、AdobeI/Oに対する認証を求めるWebブラウザが生成される場合があります。ログインしている場合は、 [必要なAdobeサービスと製品に関連付けられたAdobe資格情報を提供します](../set-up/accounts-and-services.md)。 ログインできない場合は、次の手順に従ってプロジェクト [を生成してください](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __組織の選択__
   + AEMをCloud Serviceとして持つAdobe組織を選択します。Project Fireflyは次の場所に登録されます。
1. __プロジェクトを選択__
   + プロジェクトを探して選択します。 これは、Fireflyプロジェクトテンプレートから [作成されたプロジェクトタイトル](../set-up/firefly.md) です。この場合 `WKND AEM Asset Compute`
1. __ワークスペースの選択__
   + ワークス `Development` ペースの選択
1. __このプロジェクトで有効にするAdobeI/Oアプリ機能を選択してください。 含めるコンポーネントの選択__
   +  `Actions: Deploy runtime actions`
   + 矢印キーを使用して選択と選択を解除/選択の間隔を空け、Enterキーを使用して選択を確認します
1. __生成するアクションのタイプを選択__
   +  `Adobe Asset Compute Worker`
   + 矢印キーを使用して選択し、選択を解除/選択します。選択を確認するにはEnterキーを使用します
1. __このアクションの名前を指定してください。__
   + デフォルト名を使用し `worker`ます。
   + プロジェクトに、異なる資産計算を実行する複数のワーカーが含まれる場合、意味的に名前を付けます

## console.jsonを生成

新しく作成したアセット計算プロジェクトのルートから、次のコマンドを実行してを生成し `console.json`ます。

```
$ aio app use
```

現在のワークスペースの詳細が正しいことを確認し、プリティ `Y` またはEnterを押してを生成し `console.json`ます。 とが既に存在し `.env` ていることが検出された場合は、をタップ `.aio``x` して作成をスキップします。

## プロジェクトの構造を確認する

生成されるアセット計算プロジェクトは、特殊なAdobeのプロジェクトFireflyプロジェクト用のNode.jsプロジェクトです。以下は、アセット計算プロジェクトに固有のものです。

+ `/actions` にはサブフォルダが含まれ、各サブフォルダは[アセット計算ワーカ]を定義します。
   + `/actions/<worker-name>/index.js` は、このワーカーの作業を実行するために実行するJavaScriptを定義します。
      + フォルダー名 `worker` はデフォルトで、に登録されている限り任意の名前を指定でき `manifest.yml`ます。
      + 必要に応じて、複数のワーカーフォルダを定義でき `/actions` ますが、で登録する必要があり `manifest.yml`ます。
+ `/test/asset-compute` には、各ワーカーのテストスイートが含まれます。 フォルダと同様に、 `/actions` には複数のサブフォルダを含めることがで `/test/asset-compute` き、それぞれがテストするワーカーに対応しています。
   + `/test/asset-compute/worker`は、特定のワーカーのテストスイートを表し、特定のテストケースを表すサブフォルダーと、テスト入力、パラメーター、期待出力が含まれます。
+ `/build` には、「Asset Compute」テストケース実行の出力、ログおよびアーティファクトが含まれます。
+ `/manifest.yml` プロジェクトが提供するアセット計算ワーカーを定義します。 AEMがCloud Serviceとして使用できるようにするには、各ワーカー実装をこのファイルに列挙する必要があります。
+ `/console.json` adobeI/O構成の定義
   + このファイルは、 `aio app use` コマンドを使用して生成/更新できます。
+ `/.aio` に、aio CLIツールで使用される設定を示します。
   + このファイルは、 `aio app use` コマンドを使用して生成/更新できます。
+ `/.env` 構文で環境変数を定義し、共有しないシークレットを `key=value` 含めます。 このファイルは生成できます。または、これらのシークレットを保護するために、このファイルをGitにチェックインしないでください。プロジェクトのデフォルト `.gitignore` ファイルを使用して無視されます。
   + このファイルは、 `aio app use` コマンドを使用して生成/更新できます。
   + このファイルで定義されている変数は、コマンドラインで変数を [書き出すことで上書きできます](../deploy/runtime.md) 。

プロジェクト構造のレビューの詳細については、「Adobeプロジェクトの [構造」プロジェクトを参照してください](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

開発の大部分は、開発中のワーカーの実装 `/actions` フォルダ、およびカスタムアセットコンピューティングワーカーのテスト `/test/asset-compute` の作成で行われます。

## Githubでのアセット計算プロジェクト

最終的なアセット計算プロジェクトは、次の場所でGithubで利用できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Githubにはプロジェクトの最後の状態が含まれ、ワーカーとテストケースが完全に入力されますが、資格情報は含まれません。 `.env`、 `console.json` または `.aio`。_


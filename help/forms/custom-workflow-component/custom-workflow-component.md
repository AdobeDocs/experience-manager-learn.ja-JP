---
title: フォームの添付ファイルをファイルシステムに保存するためのワークフローコンポーネントを作成する
description: カスタムワークフローコンポーネントを使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 10%

---

# カスタムワークフローコンポーネント

このチュートリアルは、カスタムワークフローコンポーネントの作成が必要なAEM Formsのお客様向けです。 前の手順で記述したコードを実行するように、ワークフローコンポーネントが設定されます。 ワークフローコンポーネントには、コードに対してプロセス引数を指定する機能があります。 この記事では、コードに関連付けられたワークフローコンポーネントについて説明します。


[カスタムワークフローコンポーネントのダウンロード](assets/saveFiles.zip)
ワークフローコンポーネントをインポート [パッケージマネージャーの使用](http://localhost:4502/crx/packmgr/index.jsp)

カスタムワークフローコンポーネントは、/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles にあります。

SaveFiles ノードを選択し、そのプロパティを確認します。

**componentGroup**  — このプロパティの値によって、ワークフローコンポーネントのカテゴリが決まります。

**jcr:Title**  — これは、ワークフローコンポーネントのタイトルです。

**sling:resourceSuperType** このプロパティの値によって、このコンポーネントの継承が決まります。 この場合、プロセスコンポーネントから継承します


![component-properties](assets/component-properties1.png)

## cq:dialog

作成者はダイアログを使用してコンポーネントとやり取りできます。cq:dialog は SaveFiles ノードの下にあります。
![cq-dialog](assets/cq-dialog.png)

items ノードの下のノードは、作成者がコンポーネントとやり取りする際に使用するコンポーネントのタブを表します。 共通タブとプロセスタブは非表示です。 「共通」タブと「引数」タブが表示されます。

プロセスのプロセス引数は processargs ノードの下にあります

![process-args](assets/process-arguments.png)

以下のスクリーンショットに示すように、作成者は引数を指定します。
![workflow-component](assets/custom-workflow-component.png)

値は、metadata ノードのプロパティとして保存されます。 例えば、値 **c:\formsattachments** は、メタデータノードの saveToLocation プロパティに保存されます
![save-location](assets/save-to-location.png)

## cq:editConfig

cq:EditConfig は、プライマリタイプ cq:EditConfig と、コンポーネントルートの下に cq:editConfig という名前のノードです。コンポーネントの編集動作は、コンポーネントノード（cq:EditConfig タイプ）の下に cq:editConfig タイプの cq:editConfig ノードを追加することで設定されます

![edit-config](assets/cq-edit-config.png)

cq:formParameters（ノードタイプ nt:unstructured）：ダイアログフォームに追加するその他のパラメーターを定義します。


cq:formParameters ノードのプロパティに注意してください。
![from-parameters-properties](assets/form-parameters-properties.png)

プロパティ PROCESS の値は、ワークフローコンポーネントに関連付けられる Java コードを示します。







---
title: フォームの添付ファイルをファイルシステムに保存するワークフローコンポーネントの作成
description: カスタムワークフローコンポーネントを使用してアダプティブフォームの添付ファイルをファイルシステムに書き込む方法
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
duration: 70
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '360'
ht-degree: 100%

---

# カスタムワークフローコンポーネント

このチュートリアルは、カスタムワークフローコンポーネントの作成が必要な AEM Forms のお客様を対象としています。このワークフローコンポーネントは、前のステップで記述したコードを実行するように設定されます。 ワークフローコンポーネントには、コードに対してプロセス引数を指定する機能があります。 この記事では、コードに関連付けられたワークフローコンポーネントについて説明します。


[カスタムワークフローコンポーネントをダウンロードします。](assets/saveFiles.zip)
[パッケージマネージャーを使用して](http://localhost:4502/crx/packmgr/index.jsp)ワークフローコンポーネントを読み込みます。

カスタムワークフローコンポーネントは、/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles にあります。

SaveFiles ノードを選択し、そのプロパティを調べます。

**componentGroup** - このプロパティの値によって、ワークフローコンポーネントのカテゴリが決まります。

**jcr:Title** - ワークフローコンポーネントのタイトルです。

**sling:resourceSuperType** - このプロパティの値によって、このコンポーネントの継承が決まります。 ここでは、プロセスコンポーネントから継承します。


![component-properties](assets/component-properties1.png)

## cq:dialog

作成者は、ダイアログを使用してコンポーネントとやり取りできます。cq:dialog は SaveFiles ノードの下にあります。
![cq-dialog](assets/cq-dialog.png)

items ノードの下にあるノードは、作成者がコンポーネントとのやり取りに使用するコンポーネントのタブを表します。 common タブと process タブは非表示です。 processcommon タブと processargs タブは表示されます。

プロセスのプロセス引数は processargs ノードの下にあります。

![process-args](assets/process-arguments.png)

以下のスクリーンショットに示すように、作成者が引数を指定します。
![workflow-component](assets/custom-workflow-component.png)

値は、metaData ノードのプロパティとして保存されます。 例えば、値 **c:\formsattachments** は、metaData ノードの saveToLocation プロパティに保存されます。
![save-location](assets/save-to-location.png)

## cq:editConfig

cq:EditConfig は、プライマリタイプが cq:EditConfig で名前が cq:editConfig の、コンポーネントルート下のノードです。
コンポーネントの編集動作は、cq:EditConfig タイプの cq:editConfig ノードをコンポーネントノード（cq:Component タイプ）の下に追加することで設定されます。

![edit-config](assets/cq-edit-config.png)

cq:formParameters（ノードタイプ nt:unstructured）：ダイアログフォームに追加されるその他のパラメーターを定義します。


cq:formParameters ノードのプロパティに注意してください。
![from-parameters-properties](assets/form-parameters-properties.png)

プロパティ PROCESS の値は、ワークフローコンポーネントに関連付けられる Java コードを示します。

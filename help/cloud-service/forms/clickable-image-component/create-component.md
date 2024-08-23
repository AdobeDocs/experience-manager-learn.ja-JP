---
title: クリック可能な画像コンポーネントの作成
description: AEM Forms Cloud Serviceでのクリック可能な画像コンポーネントの作成
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# コンポーネントを作成

この記事では、AEM Forms CS 向け開発に関する経験があることを前提としています。また、AEM Forms アーキタイププロジェクトを作成していることも前提としています。

IntelliJ または任意の他の任意の IDE でAEM Forms プロジェクトを開きます。 の下に svg という新しいノードを作成します。

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent`` は、Maven プロジェクトの作成時に指定された appId です。 この appId は、お使いの環境で異なる可能性があります。


## .content.xml ファイルの作成

svg ノードの下に.content.xml というファイルを作成します。 次の内容を、新しく作成したファイルに追加します。 必要に応じて、jcr:description、jcr:title および componentGroup を変更できます。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## svg.html を作成

svg.html という名前のファイルを作成します。 このファイルは USA マップのSVGをレンダリングします。[svg.html](assets/svg.html) の内容を、新しく作成したファイルにコピーします。 あなたがコピーしたのは米国の地図のSVGです。 ファイルを保存します。

## プロジェクトのデプロイ

コンポーネントをテストするには、プロジェクトをローカルクラウド対応インスタンスにデプロイします。

プロジェクトをデプロイするには、コマンドプロンプトウィンドウでプロジェクトのルートフォルダーに移動し、次のコマンドを実行する必要があります。

```
mvn clean install -PautoInstallSinglePackage
```

これにより、プロジェクトがローカルのAEM Forms インスタンスにデプロイされ、コンポーネントをアダプティブフォームに含めることができます

![usa-map](./assets/usa-map.png)
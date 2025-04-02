---
title: AEM Forms での監視フォルダーの使用
description: AEM Forms での監視フォルダーの設定と使用
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 86
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '406'
ht-degree: 100%

---

# AEM Forms での監視フォルダーの使用{#using-watched-folders-in-aem-forms}

管理者は、ネットワークフォルダーを監視フォルダーとして設定することにより、ユーザーが任意のファイル（例えば PDF ファイル）を監視フォルダーに追加した時点から、事前に設定されたワークフロー、サービス、スクリプトの操作を開始し、追加されたファイルを処理することができます。指定された操作をサービスが実行した後、結果ファイルを指定された出力フォルダーに保存します。ワークフロー、サービスおよびスクリプトについて詳しくは、こちらを参照してください。

監視フォルダーの作成について詳しくは、[こちら](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html?lang=ja)をクリックしてください。

監視フォルダーは、バッチモードでドキュメントを生成するために使用されます。 監視フォルダーのメカニズムを使用すると、印刷チャネル用のインタラクティブ通信を生成したり、出力サービスを使用してデータをテンプレートと結合したりできます。

この記事では、監視フォルダーメカニズムを通じて出力サービスを使用してデータをテンプレートと結合するユースケースについて説明します。

出力サービスは、AEM ドキュメントサービスの一部を成す OSGi サービスです。出力サービスは、様々な出力形式や、AEM Forms Designer の出力設計機能をサポートしています。出力サービスでは、XFA テンプレートと XML データを変換して、様々な形式の印刷ドキュメントを生成することができます。

出力サービスについて詳しくは、[ここをクリックしてください](https://helpx.adobe.com/jp/aem-forms/6/output-service.html)。
システム上に監視フォルダーを設定するには、次の手順に従ってください。
* [zip ファイルをダウンロードして、その内容を抽出します](assets/outputservicewatchedfolderkt.zip)。この zip ファイルには、監視フォルダーメカニズムを使用して出力サービスをテストするための監視フォルダーとサンプルファイルを作成するためのパッケージが含まれています。
   * Windows システム

      * パッケージマネージャーを使用して、outputservicewatchedfolder.zip を AEM に読み込みます。
      * これにより、outputservicewatchedfolder という監視フォルダーが C ドライブに作成されます。
   * Windows 以外のシステム
      * [監視フォルダーの設定を開きます](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)。
      * 出力サービスノードのフォルダーパスプロパティが適切な場所を指すように設定します。
      * 変更を保存します。
      * 上記の場所は監視フォルダーです。

SamplePdfFile フォルダーと SampleXdpFile フォルダーを監視フォルダーの入力フォルダーにドロップします。 ファイルが正常に処理されると、結果は監視フォルダーの結果フォルダーに配置されます。


>[!NOTE]
>
>監視フォルダーに関連付けられたスクリプトに複数のファイルが必要な場合は、フォルダーを作成し、必要なファイルをすべてそのフォルダーに配置して、そのフォルダーを監視フォルダーの入力フォルダーにドロップする必要があります。
>
>監視フォルダーに関連付けられたスクリプトに必要な入力ファイルが 1 つだけの場合は、そのファイルを監視フォルダーの入力フォルダーに直接ドロップできます。

---
title: AEM Formsでの監視フォルダーの使用
seo-title: AEM Formsでの監視フォルダーの使用
description: AEM Formsの監視フォルダーの設定と使用
seo-description: AEM Formsの監視フォルダーの設定と使用
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 20%

---


# AEM Forms{#using-watched-folders-in-aem-forms}での監視フォルダーの使用

管理者は、ネットワークフォルダーを監視フォルダーとして設定することにより、ユーザーが任意のファイル（例えば PDF ファイル）を監視フォルダーに追加した時点から、事前に設定されたワークフロー、サービス、またはスクリプティング操作を開始し、追加されたファイルを処理することができます。指定された操作をサービスが実行した後、指定された出力フォルダーに出力ファイルが保存されます。ワークフロー、サービス、スクリプトの詳細を参照してください。

監視フォルダーの作成の詳細については、[ここをクリック](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)してください

監視フォルダーは、バッチモードでドキュメントを生成するために使用します。 監視チャネルーのメカニズムを使用して、印刷フォルダー用のインタラクティブ通信を生成したり、Outputサービスを使用してデータをテンプレートとマージしたりできます。

この記事では、監視フォルダーメカニズムを使用した出力サービスを使用して、データとテンプレートを結合する使用例を説明します。

出力サービスは、AEM ドキュメントサービスの一部である OSGi サービスの一種です。Outputサービスは、AEM Formsデザイナーの様々な出力形式と出力設計機能をサポートしています。 出力サービスでは、XFA テンプレートと XML データを変換することにより、様々な形式の印刷ドキュメントを生成することができます。

出力サービスの詳細については、[ここをクリック](https://helpx.adobe.com/aem-forms/6/output-service.html)してください。
システム上に監視フォルダーを設定するには、次の手順に従います。
* [zipファイルの内容をダウンロードして抽出します](assets/outputservicewatchedfolderkt.zip)。このzipファイルには、監視フォルダーメカニズムを使用して出力サービスをテストするための監視フォルダーとサンプルファイルを作成するパッケージが含まれています
   * Windowsシステム

      * パッケージマネージャーを使用して、outputservicewatchedfolder.zipをAEMに読み込みます
      * これにより、Cドライブにoutputservicewatchedfolderという名前の監視フォルダーが作成されます。
   * Windows以外のシステム
      * [監視フォルダーの設定を開きます](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 適切な場所を指すようにoutserviceノードのフォルダーパスプロパティを設定します
      * 変更を保存する
      * 上述の場所は監視フォルダーです。

SamplePdfFileフォルダーとSampleXdpFileフォルダーを監視フォルダーの入力フォルダーにドロップします。 ファイルが正常に処理されると、結果は監視フォルダーの結果フォルダーに配置されます。


>[!NOTE]
>
>監視フォルダーに関連付けられたスクリプトに複数のファイルが必要な場合は、フォルダーを作成し、必要なファイルをすべてフォルダーに配置して、フォルダーを監視フォルダーの入力フォルダーにドロップする必要があります。
>
>監視フォルダーに関連付けられたスクリプトに必要な入力ファイルが1つだけの場合は、そのファイルを直接監視フォルダーの入力フォルダーにドロップできます


---
title: AEM Formsでの監視フォルダーの使用
seo-title: AEM Formsでの監視フォルダーの使用
description: AEM Formsでの監視フォルダーの設定と使用
seo-description: AEM Formsでの監視フォルダーの設定と使用
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: Output サービス
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 20%

---


# AEM Forms{#using-watched-folders-in-aem-forms}での監視フォルダーの使用

管理者は、ネットワークフォルダーを監視フォルダーとして設定することにより、ユーザーが任意のファイル（例えば PDF ファイル）を監視フォルダーに追加した時点から、事前に設定されたワークフロー、サービス、またはスクリプティング操作を開始し、追加されたファイルを処理することができます。指定された操作をサービスが実行した後、指定された出力フォルダーに出力ファイルが保存されます。ワークフロー、サービス、スクリプトについて詳しくは、

監視フォルダーの作成について詳しくは、[ここをクリック](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)してください。

監視フォルダーは、バッチモードでドキュメントを生成するために使用されます。 監視フォルダーのメカニズムを使用して、印刷チャネル用のインタラクティブ通信を生成したり、出力サービスを使用してデータをテンプレートとマージしたりできます。

この記事では、監視フォルダーメカニズムを使用した出力サービスを使用したテンプレートとのデータの結合の使用例について説明します。

出力サービスは、AEM ドキュメントサービスの一部である OSGi サービスの一種です。Outputサービスは、AEM Forms Designerの様々な出力形式と出力デザイン機能をサポートしています。 出力サービスでは、XFA テンプレートと XML データを変換することにより、様々な形式の印刷ドキュメントを生成することができます。

出力サービスの詳細については、[こちら](https://helpx.adobe.com/aem-forms/6/output-service.html)をクリックしてください。
システム上に監視フォルダーを設定するには、次の手順に従います。
* [zipファイルの内容をダウンロードして抽出します](assets/outputservicewatchedfolderkt.zip)。このzipファイルには、監視フォルダーを作成するためのパッケージと、監視フォルダーメカニズムを使用して出力サービスをテストするためのサンプルファイルが含まれています
   * Windowsシステム

      * パッケージマネージャーを使用して、outputservicewatchedfolder.zipをAEMに読み込みます。
      * これにより、Cドライブにoutputservicewatchedfolderという監視フォルダーが作成されます。
   * Windows以外のシステム
      * [監視フォルダーの設定を開く](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 送信サービスノードのフォルダーパスプロパティが適切な場所を指すように設定します
      * 変更を保存します
      * 上記の場所は監視フォルダーです。

SamplePdfFileフォルダーとSampleXdpFileフォルダーを監視フォルダーの入力フォルダーにドロップします。 ファイルが正常に処理されると、結果は監視フォルダーの結果フォルダーに配置されます。


>[!NOTE]
>
>監視フォルダーに関連付けられたスクリプトに複数のファイルが必要な場合は、フォルダーを作成し、必要なファイルをすべてフォルダーに配置して、監視フォルダーの入力フォルダーにフォルダーをドロップする必要があります。
>
>監視フォルダーに関連付けられたスクリプトで必要な入力ファイルが1つだけの場合は、そのファイルを監視フォルダーの入力フォルダーに直接ドロップできます


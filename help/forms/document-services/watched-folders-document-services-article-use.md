---
title: AEM Formsでの監視フォルダーの使用
description: AEM Formsでの監視フォルダーの設定と使用
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 24%

---

# AEM Formsでの監視フォルダーの使用{#using-watched-folders-in-aem-forms}

管理者は、ネットワークフォルダーを監視フォルダーとして設定することにより、ユーザーが任意のファイル（例えば PDF ファイル）を監視フォルダーに追加した時点から、事前に設定されたワークフロー、サービス、またはスクリプティング操作を開始し、追加されたファイルを処理することができます。指定された操作をサービスが実行した後、指定された出力フォルダーに出力ファイルが保存されます。ワークフロー、サービス、スクリプトについて詳しくは、こちらを参照してください。

監視フォルダーの作成について詳しくは、以下を参照してください。 [ここをクリック](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

監視フォルダーは、バッチモードでドキュメントを生成するために使用されます。 監視フォルダーのメカニズムを使用して、印刷チャネル用のインタラクティブ通信を生成したり、Output サービスを使用してデータをテンプレートと結合したりできます。

この記事では、監視フォルダーメカニズムを使用した出力サービスを使用してデータをテンプレートと結合する使用例について説明します。

出力サービスは、AEM ドキュメントサービスの一部である OSGi サービスの一種です。出力サービスは、様々な出力形式や、AEM Forms Designer の出力設計機能をサポートしています。出力サービスでは、XFA テンプレートと XML データを変換することにより、様々な形式の印刷ドキュメントを生成することができます。

Output サービスの詳細を確認するには、 [ここをクリックしてください](https://helpx.adobe.com/aem-forms/6/output-service.html).
システム上に監視フォルダーを設定するには、次の手順に従ってください。
* [zip ファイルの内容をダウンロードして抽出します。](assets/outputservicewatchedfolderkt.zip).この zip ファイルには、監視フォルダーを作成するためのパッケージと、監視フォルダーメカニズムを使用して出力サービスをテストするためのサンプルファイルが含まれています
   * Windows システム

      * パッケージマネージャーを使用して、outputservicewatchedfolder.zip をAEMに読み込みます。
      * これにより、C ドライブに outputservicewatchedfolder という監視フォルダーが作成されます。
   * Windows 以外のシステム
      * [監視フォルダーの設定を開きます](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 送信サービスノードのフォルダーパスプロパティが適切な場所を指すように設定します
      * 変更を保存します
      * 上記の場所は監視フォルダーです。

SamplePdfFile フォルダーと SampleXdpFile フォルダーを監視フォルダーの入力フォルダーにドロップします。 ファイルが正常に処理されると、結果は監視フォルダーの結果フォルダーに配置されます。


>[!NOTE]
>
>監視フォルダーに関連付けられたスクリプトに複数のファイルが必要な場合は、フォルダーを作成し、必要なファイルをすべてフォルダーに配置して、そのフォルダーを監視フォルダーの入力フォルダーにドロップする必要があります。
>
>監視フォルダーに関連付けられたスクリプトで必要な入力ファイルが 1 つだけの場合は、そのファイルを監視フォルダーの入力フォルダーに直接ドロップできます

---
title: 監視フォルダーを使用した印刷チャネルドキュメントの生成
seo-title: 監視フォルダーを使用した印刷チャネルドキュメントの生成
description: これは、印刷チャネル用の最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの10部分です。 ここでは、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成します。
seo-description: これは、印刷チャネル用の最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの10部分です。 ここでは、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成します。
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: 対話型通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---


# 監視フォルダーを使用した印刷チャネルドキュメントの生成

ここでは、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成します。

印刷チャネルドキュメントを作成してテストした後、バッチモードまたはオンデマンドでこれらのドキュメントを生成するメカニズムが必要です。 通常、この種のドキュメントはバッチモードで生成され、最も一般的なメカニズムは監視フォルダーを使用するものです。

AEMで監視フォルダーを設定する場合、ファイルが監視フォルダーに配置されたときに実行されるECMAスクリプトまたはJavaコードを関連付けます。 この記事では、印刷チャネルドキュメントを生成してファイルシステムに保存するECMAスクリプトについて説明します。

監視フォルダーの設定とECMAスクリプトは、このチュートリアル](introduction.md)の[先頭で読み込んだアセットの一部です

監視フォルダーに配置される入力ファイルの構造は次のとおりです。 ECMAスクリプトは、アカウント番号を読み取り、これらの各アカウントの印刷チャネルドキュメントを生成します。

ドキュメントを生成するためのECMAスクリプトの詳細については、[この記事](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)を参照してください。

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

監視チャネルーのメカニズムを使用して印刷ドキュメントを生成するには、次の手順に従ってください。

* [このドキュメントで説明する手順に従います](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* crxにログインし、/etc/fd/watchfolder/scripts/PrintPDF.ecmaに移動します。

* interactiveCommunicationsDocumentへのパスが、印刷する正しいドキュメントを指していることを確認してください。（ 1行目）
* saveLocation（2行目）をメモしておきます。必要に応じて変更できます。
* フォームデータモデルの入力パラメーターがRequest属性にバインドされ、その連結値が「accountnumber」に設定されていることを確認します。 以下のスクリーンショットを参照してください。
   ![request](assets/requestattributeprintchannel.gif)

* 次の内容のaccountnumbers.xmlファイルを作成します

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* xmlファイルをC:\RenderPrintChannel\inputにドロップします。

* ECMAスクリプトで指定した保存場所のpdfファイルを確認します。





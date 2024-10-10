---
title: 監視フォルダーを使用して印刷チャネルドキュメントを生成する
description: これは、印刷チャネル用の最初のインタラクティブコミュニケーションドキュメントを作成するための。マルチステップチュートリアルの第 10 部です。 ここでは、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成します。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 70
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '351'
ht-degree: 100%

---

# 監視フォルダーを使用して印刷チャネルドキュメントを生成する

ここでは、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成します。

印刷チャネルドキュメントを作成しテストした後、バッチモードまたはオンデマンドでこれらのドキュメントを生成するメカニズムが必要です。 通常、この種のドキュメントはバッチモードで生成され、最も一般的なメカニズムでは監視フォルダーを使用します。

AEM で監視フォルダーを設定する場合、監視フォルダーにファイルをドロップする際に実行される ECMA スクリプトまたは Java コードを関連付けます。 この記事では、印刷チャネルドキュメントを生成してファイルシステムに保存する ECMA スクリプトについて説明します。

監視フォルダー設定と ECMA スクリプトは、[このチュートリアルの最初](introduction.md)に読み込んだアセットの一部です。

監視フォルダーにドロップされる入力ファイルは、次のような構造になっています。ECMA スクリプトは、アカウント番号を読み取り、これらのアカウントごとに印刷チャネルドキュメントを生成します。

ドキュメント生成用の ECMA スクリプトについて詳しくは、[この記事を参照してください](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成するには、次の手順に従ってください。

* [このドキュメントで説明されている手順に従います](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* crx にログインし、/etc/fd/watchfolder/scripts/PrintPDF.ecma に移動します

* interactiveCommunicationsDocument へのパスが、印刷する正しいドキュメントを指していることを確認します。（1 行目）
* saveLocation（2 行目）をメモしておきます。必要に応じて変更できます。
* フォームデータモデルへの入力パラメーターがリクエスト属性にバインドされ、そのバインド値が「accountnumber」に設定されていることを確認します。 以下のスクリーンショットを参照してください。
  ![リクエスト](assets/requestattributeprintchannel.gif)

* 次の内容の ccountnumbers.xml ファイルを作成します

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

* xml ファイルを C:\RenderPrintChannel\input にドロップします

* ECMA スクリプトで指定したように、保存場所の PDF ファイルを確認します。

## 次の手順

[フォーム送信時にエージェント UI を開く](./opening-agent-ui-on-form-submission.md)
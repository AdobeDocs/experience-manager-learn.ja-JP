---
title: 大きな XML データファイルを XDP テンプレートと結合する際のタイムアウトエラーの修正
description: AEM Forms での大きな XML ファイルとテンプレートの結合
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 37
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '178'
ht-degree: 100%

---

# 大きな XML データファイルと XDP テンプレートを結合して PDF ファイルを作成できるようにする方法

大きな XML データファイルを XDP テンプレートと結合すると、ログファイルに次のエラーが表示される場合があります。

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

上記のエラーを修正するには、次の手順を実行します。

## Aries タイムアウトの変更

* AEM サーバーを停止します。
* インストールした AEM の crx-quickstart フォルダーの下に、**install** という名前のフォルダーを作成します。
* **org.apache.aries.transaction.config** というファイルをインストールフォルダーに作成し、次の内容を設定します。
aries.transaction.timeout=&quot;1200&quot;
このタイムアウト値は、必要に応じて変更できます。タイムアウト値は秒単位です。

>[!NOTE]
> org.apache.aries.transaction 設定を作成したら、ファイルを編集する代わりに、[configMgr](http://localhost:4502/system/console/configMgr) からトランザクションタイムアウトの値を編集することができます。


## Jacorb ORB プロバイダー設定の変更

* [OSGi ConfigMgr を開きます](http://localhost:4502/system/console/configMgr)。
* **Jacorb ORB Provider** を検索します。
* 次のエントリを追加します。
jacorb.connection.client.pending_reply_timeout=600000
この設定は、保留中の応答のタイムアウト（CORBA クライアントタイムアウト）を 600 秒に設定しています。
* 変更を保存します。

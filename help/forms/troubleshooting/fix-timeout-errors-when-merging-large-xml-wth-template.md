---
title: 大きな xml データファイルを xdp テンプレートと結合する際のタイムアウトエラーを修正しました
description: 大きな xml ファイルをAEM Formsのテンプレートと結合
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# 大きな xml データファイルと xdp テンプレートを結合して pdf ファイルの作成を有効にする方法

大きな xml データファイルを xdp テンプレートと結合すると、ログファイルに次のエラーが表示される場合があります

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

上記のエラーを修正するには、次の手順を実行します

## 並列タイムアウトを変更

* AEM Server を停止
* という名前のフォルダーを作成します。 **インストール** AEMインストールの crx-quickstart フォルダーの下
* という名前のファイルを作成します。 **org.apache.aries.transaction.config** install フォルダーの下に次のコンテンツ aries.transaction.timeout=&quot;1200&quot;があります。 必要に応じて、タイムアウト値を変更できます。 タイムアウト値は秒単位です

>[!NOTE]
> org.apache.aries.transaction 設定を作成したら、 [configMgr](http://localhost:4502/system/console/configMgr) ファイルを編集する代わりに


## Jacorb ORB プロバイダ設定を変更する

* [OSGi ConfigMgr を開きます。](http://localhost:4502/system/console/configMgr)
* を検索 **Jacorb ORB プロバイダ**
* 以下のエントリ jacorb.connection.client.pending_reply_timeout=600000を追加します。上記の設定は、保留中の応答タイムアウト（CORBA クライアントタイムアウト）を 600 秒に設定します。
* 変更を保存します

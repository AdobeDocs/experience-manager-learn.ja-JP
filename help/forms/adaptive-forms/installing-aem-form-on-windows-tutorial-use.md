---
title: WindowsへのAEM Formsのインストール手順の簡略化
seo-title: WindowsへのAEM Formsのインストール手順の簡略化
description: WindowsにAEM Formsを簡単にインストールする手順
seo-description: WindowsにAEM Formsを簡単にインストールする手順
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: アダプティブフォーム
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 8%

---


# WindowsへのAEM Formsのインストール手順の簡略化

>[!NOTE]
>
>AEM Formsを使用する場合は、AEM Quick Start jarをダブルクリックしないでください。
>
>また、AEM Forms Installationフォルダーのパスにスペースがないことを確認します。
>
>例えば、 c:\jack and jill\AEM Forms folderにAEM Formsをインストールしないでください。

>[!NOTE]
>
>AEM Forms 6.5をインストールする場合は、次の32ビットMicrosoft Visual C++再頒布可能パッケージがインストールされていることを確認してください。
>
>* Microsoft Visual C++ 2008再頒布可能パッケージ
>* Microsoft Visual C++ 2010再頒布可能パッケージ
>* Microsoft Visual C++ 2012再頒布可能パッケージ
>* Microsoft Visual C++ 2013の再頒布可能パッケージ（6.5時点）


AEM Formsのインストールに関する[公式ドキュメント](https://helpx.adobe.com/jp/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)に従うことをお勧めします。 次の手順に従って、Windows環境にAEM Formsをインストールして設定できます。

* 適切なJDKがインストールされていることを確認します。
   * AEM 6.2には以下が必要です。OracleSE 8 JDK 1.8.x（64ビット）
* 
   * AEM 6.3およびAEM 6.4には、以下が必要です。OracleSE 8 JDK 1.8.x（64ビット）
* AEM 6.5にはJDK 8またはJDK 11が必要
* [公式JDK要件を](https://helpx.adobe.com/jp/experience-manager/6-3/sites/deploying/using/technical-requirements.html) 以下に示します
* JAVA_HOMEが、インストールしたJDKを指すように設定されていることを確認します。
   * WindowsでJAVA_HOME変数を作成するには、次の手順に従います。
      * 「マイコンピューター」を右クリックし、「プロパティ」を選択します。
      * 「詳細」タブで、「環境変数」を選択し、JAVA_HOMEという新しいシステム変数を作成します。
      * システムにインストールされているJDKを指すように変数値を設定します。 例： c:\program files\java\jdk1.8.0_25

* CドライブにAEM Formsというフォルダーを作成します
* AEM QuickStart.Jarを探し、AEM Formsフォルダーに移動します
* このAEM Formsフォルダーにlicense.propertiesファイルをコピーします。
* 次の内容の「StartAemForms.bat」というバッチファイルを作成します。
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.ここで、AEM_6.3_Quickstart.jarはAEMのクイックスタートjarの名前です。
   * jarの名前は任意の名前に変更できますが、名前がバッチファイルに反映されていることを確認してください。バッチファイルをAEM Formsフォルダーに保存してください。

* 新しいコマンドプロンプトを開き、 c:\aemformsに移動します。

* コマンドプロンプトからStartAemForms.batファイルを実行します。

* 起動の進行状況を示す小さなダイアログボックスが表示されます。

* 起動が完了したら、 sling.propertiesファイルを開きます。 これは、c:\AEMForms\crx-quickstart\conf folderにあります。

* 次の2行をファイルの末尾にコピーします。
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider= org.bouncycastle.***
* ドキュメントサービスを動作させるには、次の2つのプロパティが必要です
* sling.propertiesファイルを保存します。

* [パッケージ共有にログイン](http://localhost:4502/crx/packageshare/login.html)

   * パッケージ共有にログインするには、Adobe IDが必要です
   * お使いのバージョンのAEM Formsおよびオペレーティングシステムに適したAEM Formsアドオンパッケージを検索します。
   * または、[適切なフォームアドオンパッケージ](https://helpx.adobe.com/jp/aem-forms/kb/aem-forms-releases.html)をダウンロードできます。
   * アドオンパッケージをインストールしたら、次の手順に従う必要があります

      * **すべてのバンドルがアクティブ状態であることを確認します。（AEMFD Signaturesバンドルを除く）.**
      * **通常、すべてのバンドルがアクティブ状態になるまでに5分以上かかります。**
   * **すべてのバンドルがアクティブになったら（AEMFD Signaturesバンドルを除く）、システムを再起動してAEM Formsのインストールを完了します**


* `sun.util.calendar`パッケージを許可リストに追加：

   1. [ブラウザーウィンドウ](http://localhost:4502/system/console/configMgr)でFelix Webコンソールを開きます。
   2. デシリアライゼーションファイアウォール設定を検索して開きます。`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. `sun.util.calendar`を`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`の下の新しいエントリとして追加します。
   4. 変更内容を保存します。

これで完了です!!! これで、システムにAEM Formsがインストールされ、設定されました。
必要に応じて、サーバー上の[Reader拡張](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)または[ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html)を設定できます

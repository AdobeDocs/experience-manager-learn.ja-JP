---
title: WindowsでのAEM Formsのインストールの簡単な手順
seo-title: WindowsでのAEM Formsのインストールの簡単な手順
description: WindowsにAEM Formsを簡単にインストールする手順
seo-description: WindowsにAEM Formsを簡単にインストールする手順
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 82127d5be9a4b969537738f9ba537efe07f38479
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 6%

---

# WindowsでのAEM Formsのインストールの簡単な手順

>[!NOTE]
>AEM Formsを使用する場合は、重複がAEMクイック開始jarをクリックしないでください。
>また、[AEM Formsインストール]フォルダのパスにスペースがないことを確認します。
>例えば、c:\jack and jill\AEM Forms folderにAEM Formsをインストールしないでください。

>[!NOTE]
AEM Forms6.5をインストールする場合は、次の32ビット版Microsoft Visual C++再配布可能ファイルがインストールされていることを確認してください。

* Microsoft Visual C++ 2008再配布可能
* Microsoft Visual C++ 2010再配布可能
* Microsoft Visual C++ 2012再配布可能
* Microsoft Visual C++ 2013再配布可能（6.5以降）

AEM Formsの設置に関する [公式文書に従うことをお勧めします](https://helpx.adobe.com/jp/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) 。 Windows環境にAEM Formsをインストールして設定するには、次の手順に従います。

* 適切なJDKがインストールされていることを確認します
   * AEM 6.2が必要：Oracle SE 8 JDK 1.8.x (64bit)
* 
   * AEM 6.3およびAEM 6.4が必要：Oracle SE 8 JDK 1.8.x (64bit)
* AEM 6.5 JDK 8またはJDK 11が必要
* [公式JDKの要件](https://helpx.adobe.com/jp/experience-manager/6-3/sites/deploying/using/technical-requirements.html) :
* JAVA_HOMEが、インストール済みのJDKを指すように設定されていることを確認します。
   * WindowsでJAVA_HOME変数を作成するには、次の手順に従います。
      * 「マイコンピューター」を右クリックし、「プロパティ」を選択します。
      * 「詳細」タブで、「環境変数」を選択し、JAVA_HOMEという名前の新しいシステム変数を作成します。
      * 変数値を、システムにインストールされているJDKを示すように設定します。 例：c:\program files\java\jdk1.8.0_25

* Cドライブ上にAEMFormsという名前のフォルダーを作成する
* AEMQuickStart.Jarを探し、AEMFormsフォルダーに移動します
* license.propertiesファイルをこのAEMFormsフォルダーにコピーします
* 次の内容の「StartAemForms.bat」という名前のバッチファイルを作成します。
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.ここで、AEM_6.3_Quickstart.jarはAEM quickstart jarの名前です。
   * jarの名前は任意の名前に変更できますが、名前がバッチファイルに反映されていることを確認してください。バッチファイルはAEMFormsフォルダーに保存してください。

* 新しいコマンドプロンプトを開き、c:\aemformsに移動します。

* コマンドプロンプトからStartAemForms.batファイルを実行します。

* 起動の進行状況を示す小さなダイアログボックスが表示されます。

* 起動が完了したら、sling.propertiesファイルを開きます。 これは、c:\AEMForms\crx-quickstart\conf folderにあります。

* 次の2行をファイルの下部にコピーします
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider= org.bouncycastle.***
* ドキュメントサービスが動作するには、これら2つのプロパティが必要です
* sling.propertiesファイルを保存します

* [パッケージ共有にログイン](http://localhost:4502/crx/packageshare/login.html)

   * パッケージ共有にログインするにはAdobe IDが必要です
   * お使いのバージョンのAEM Formsとオペレーティングシステムに適したパッケージで、追加AEM Formsを検索してください。
   * または、適切 [なフォームアドオンパッケージをダウンロードできます](https://helpx.adobe.com/jp/aem-forms/kb/aem-forms-releases.html)
   * アドオンパッケージをインストールしたら、次の手順に従う必要があります

      * **すべてのバンドルがアクティブ状態になっていることを確認します。 （AEMFD Signaturesバンドルを除く）。**
      * **通常、すべてのバンドルがアクティブ状態になるまで5分以上かかります。**
   * **すべてのバンドルがアクティブになったら（AEMFD Signaturesバンドルを除く）、システムを再起動してAEM Formsのインストールを完了します**


* 許可リストに追加 `sun.util.calendar` パッケージ化：

   1. ブラウザーウィンドウでFelix Webコンソールを開き [ます](http://localhost:4502/system/console/configMgr)
   2. Search and open Deserialization Firewall Configuration: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. ～ `sun.util.calendar` の新追加参入 `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. 変更内容を保存します。

おめでとう！!! これで、お使いのシステムにAEM Formsをインストールし、設定しました。
必要に応じて、サーバー上で [Reader拡張機能](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) 、 [ またはPDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) を設定できます

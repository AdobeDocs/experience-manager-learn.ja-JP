---
title: Windows にAEM Formsをインストールする手順の簡略化
description: Windows にAEM Formsを簡単にインストールする手順
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
source-git-commit: 5c53919dd038c0992e1fe5dd85053f26c03c5111
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 8%

---

# Windows にAEM Formsをインストールする手順の簡略化

>[!NOTE]
>
>AEM Formsを使用する場合は、AEM Quick Start jar をダブルクリックしないでください。
>
>また、AEM Forms Installation フォルダーのパスにスペースがないことを確認します。
>
>例えば、 c:\jack and jill\AEM Forms folderにAEM Formsをインストールしないでください。

>[!NOTE]
>
>AEM Forms 6.5 をインストールする場合は、次の 32 ビット版のMicrosoft Visual C++再配布可能パッケージがインストールされていることを確認してください。
>
>* Microsoft Visual C++ 2008 の再頒布可能パッケージ
>* Microsoft Visual C++ 2010 の再頒布可能パッケージ
>* Microsoft Visual C++ 2012 再頒布可能パッケージ
>* Microsoft Visual C++ 2013 の再頒布可能パッケージ（6.5 時点）


ただし、 [公式ドキュメント](https://helpx.adobe.com/jp/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) AEM Formsのインストール用 次の手順に従って、Windows 環境にAEM Formsをインストールして設定できます。

* 適切な JDK がインストールされていることを確認します。
   * AEM 6.2 には以下が必要です。OracleSE 8 JDK 1.8.x（64 ビット）
* 
   * AEM 6.3 およびAEM 6.4 には、以下が必要です。OracleSE 8 JDK 1.8.x（64 ビット）
* AEM 6.5 には JDK 8 または JDK 11 が必要です
* [公式 JDK の要件](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=en) ここにリストされている
* JAVA_HOME が、インストールした JDK を指すように設定されていることを確認します。
   * Windows で JAVA_HOME 変数を作成するには、次の手順に従います。
      * 「マイコンピューター」を右クリックし、「プロパティ」を選択します。
      * 「詳細設定」タブで、「環境変数」を選択し、JAVA_HOME という新しいシステム変数を作成します。
      * 変数値を、システムにインストールされた JDK を示すように設定します。 例： c:\program files\java\jdk1.8.0_25

* C ドライブ上に AEMForms というフォルダーを作成します。
* AEM QuickStart.Jar を探し、AEM Forms フォルダーに移動します。
* この AEM Forms フォルダーに license.properties ファイルをコピーします。
* 「StartAemForms.bat」という名前のバッチファイルを次の内容で作成します。
   * java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui ここで、AEM_6.5_Quickstart.jar はAEMのクイックスタート jar の名前です。
   * jar の名前は任意の名前に変更できますが、名前がバッチファイルに反映されていることを確認してください。 AEM Forms フォルダーにバッチファイルを保存します。

* 新しいコマンドプロンプトを開き、に移動します。 _c:\aemforms_.

* コマンドプロンプトから StartAemForms.bat ファイルを実行します。

* 起動の進行状況を示す小さなダイアログボックスが表示されます。

* 起動が完了したら、sling.properties ファイルを開きます。 これはc:\AEMForms\crx-quickstart\conf folderにあります。

* 次の 2 行をファイルの末尾にコピーします。
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider= org.bouncycastle.&#42;**
* ドキュメントサービスが動作するには、次の 2 つのプロパティが必要です
* sling.properties ファイルを保存します。
* [適切なフォームアドオンパッケージをダウンロードします。](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=ja)
* 次を使用してフォームアドオンパッケージをインストールする [パッケージマネージャー。](http://localhost:4502/crx/packmgr/index.jsp)
* パッケージにアドオンをインストールしたら、次の手順に従う必要があります

       * **すべてのバンドルがアクティブ状態であることを確認します。 （AEMFD Signatures バンドルを除く）。**
       * **通常、すべてのバンドルがアクティブ状態になるまでに 5 分以上かかります。**
   
   * **すべてのバンドルがアクティブになったら（AEMFD Signatures バンドルを除く）、システムを再起動してAEM Formsのインストールを完了します**

## sun.util.calendar パッケージから許可リスト

1. の Felix Web コンソールを開きます。 [ブラウザーウィンドウ](http://localhost:4502/system/console/configMgr)
2. デシリアライゼーションファイアウォール設定を検索して開きます。 `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. 追加 `sun.util.calendar` の下の新しいエントリとして `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. 変更を保存します。

おめでとう!!! これで、システムにAEM Formsをインストールし、設定しました。
必要に応じて、  [Reader拡張](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) または [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=ja) サーバーの

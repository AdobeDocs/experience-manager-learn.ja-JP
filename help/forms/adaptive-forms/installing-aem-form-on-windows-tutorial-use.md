---
title: Windows に AEM Forms をインストールする手順の簡略化
description: Windows に AEM Forms を手早く簡単にインストールする手順
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 100%

---

# Windows に AEM Forms をインストールする手順の簡略化

>[!NOTE]
>
>AEM Forms を使用する場合は、AEM Quick Start jar をダブルクリックしないでください。
>
>また、AEM Forms インストールフォルダーのパスにスペースが入っていないことを確認します。
>
>例えば、C:\jack and jill\AEM Forms フォルダーに AEM Forms をインストールしないでください。

>[!NOTE]
>
>AEM Forms 6.5 をインストールする場合は、次の 32 ビット版の Microsoft Visual C++ 再頒布可能パッケージがインストールされていることを確認してください。
>
>* Microsoft Visual C++ 2008 再頒布可能パッケージ
>* Microsoft Visual C++ 2010 再頒布可能パッケージ
>* Microsoft Visual C++ 2012 再頒布可能パッケージ
>* Microsoft Visual C++ 2013 再頒布可能パッケージ（6.5 時点）

ただし、AEM Forms のインストールについては、[公式ドキュメント](https://helpx.adobe.com/jp/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)に従うことをお勧めします。次の手順に従って、Windows 環境に AEM Forms をインストールし設定できます。

* 適切な JDK がインストールされていることを確認します。
   * AEM 6.2 には、Oracle SE 8 JDK 1.8.x（64 ビット）が必要です。
   * AEM 6.3 および AEM 6.4 には、Oracle SE 8 JDK 1.8.x（64 ビット）が必要です。
   * AEM 6.5 には JDK 8 または JDK 11 が必要です。
   * [公式の JDK 要件](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=ja)は、ここに掲載されています。
* JAVA_HOME が、インストールした JDK を指すように設定されていることを確認します。
   * Windows で JAVA_HOME 変数を作成するには、次の手順に従います。
      * 「マイコンピューター」を右クリックし、「プロパティ」を選択します。
      * 「詳細設定」タブで、「環境変数」を選択し、JAVA_HOME という新しいシステム変数を作成します。
      * 変数値を、システムにインストールされている JDK を指すように設定します。例：C:\Program Files\Java\jdk1.8.0_25

* C ドライブ上に AEMForms というフォルダーを作成します。
* AEM QuickStart.Jar を探して、AEMForms フォルダーに移動します。
* この AEMForms フォルダーに license.properties ファイルをコピーします。
* 「StartAemForms.bat」という名前のバッチファイルを次の内容で作成します。
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * ここで、AEM_6.5_Quickstart.jar は AEM のクイックスタート JAR の名前です。
   * JAR の名前は任意の名前に変更できますが、その名前がバッチファイルに反映されていることを確認してください。AEMForms フォルダーにバッチファイルを保存します。

* 新しいコマンドプロンプトを開き、_c:\aemforms_ に移動します。

* コマンドプロンプトから StartAemForms.bat ファイルを実行します。

* 起動の進行状況を示す小さなダイアログボックスが表示されます。

* 起動が完了したら、sling.properties ファイルを開きます。これは C:\AEMForms\crx-quickstart\conf フォルダーにあります。

* 次の 2 行をファイルの末尾にコピーします。
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;****sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider= org.bouncycastle.&#42;**
* ドキュメントサービスが機能するには、次の 2 つのプロパティが必要です。
* sling.properties ファイルを保存します。
* [適切な Forms アドオンパッケージをダウンロードします。](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=ja)
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、Forms アドオンパッケージをインストールします。
* アドオンパッケージをインストールしたら、次の手順に従う必要があります。

   * **すべてのバンドルがアクティブ状態であることを確認します。（AEMFD Signatures バンドルを除く）。**
   * **通常、すべてのバンドルがアクティブ状態になるまでに 5 分以上かかります。**

   * **すべてのバンドルがアクティブになったら（AEMFD Signatures バンドルを除く）、システムを再起動して AEM Forms のインストールを完了します。**

## sun.util.calendar パッケージから許可リスト

1. [ブラウザーウィンドウ](http://localhost:4502/system/console/configMgr)で Felix Web コンソールを開きます。
1. デシリアライゼーションファイアウォール設定を検索して開きます。`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name` の下に新規エントリとして `sun.util.calendar` を追加します
1. 変更を保存します。

おめでとうございます。これで、システムへの AEM Forms のインストールと設定が完了しました。
必要に応じて、サーバーで [Reader Extensions](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=ja) または [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=ja) を設定できます

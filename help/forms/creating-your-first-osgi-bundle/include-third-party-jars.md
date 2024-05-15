---
title: サードパーティ JAR の組み込み
description: AEM プロジェクトでサードパーティの JAR ファイルを使用する方法を説明します。
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 100%

---

# AEM プロジェクトへのサードパーティバンドルの組み込み

この記事では、AEM プロジェクトにサードパーティの OSGi バンドルを組み込む手順について説明します。ここでは、 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) を AEM プロジェクトに追加します。OSGi が Maven リポジトリで使用可能な場合は、プロジェクトの POM.xml ファイルにバンドルの依存関係を含めます。

>[!NOTE]
> サードパーティの JAR が OSGi バンドルであることを想定します。

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

OSGi バンドルがファイルシステム上にある場合は、プロジェクトのベースディレクトリ（C:\aemformsbundles\AEMFormsProcessStep\localjar）の下で **localjar** という名前のフォルダーを作成します。依存関係は次のようになります。

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## フォルダー構造の作成

このバンドルを AEM プロジェクト **AEMFormsProcessStep** に追加します。これは **c:\aemformsbundles** フォルダーに格納されます。

* プロジェクトの C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault フォルダーから **filter.xml** を開きます。

* フォルダー構造（C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install）を作成します。
* **apps/AEMFormsProcessStep-vendor-packages** は filter.xml のルート属性値です。
* プロジェクトの POM.xml の依存関係セクションを更新します。
* コマンドプロンプトを開きます。ここでは、プロジェクトのフォルダー（c:\aemformsbundles\AEMFormsProcessStep）に移動します。 以下のコマンドを実行します。

```java
mvn clean install -PautoInstallSinglePackage
```

問題がなければ、パッケージがサードパーティバンドルと共に AEM インスタンスにインストールされます。 バンドルは [Felix web コンソール](http://localhost:4502/system/console/bundles)を使用して確認できます。サードパーティバンドルは、次に示す `crx` リポジトリの /apps フォルダーにあります。
![サードパーティ](assets/custom-bundle1.png)

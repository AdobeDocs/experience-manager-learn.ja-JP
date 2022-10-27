---
title: AEM Formsでの最初の OSGi バンドルの作成
description: Maven と Eclipse を使用して最初の OSGi バンドルを構築
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: 11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 2%

---

# AEMプロジェクトへのサードパーティバンドルの追加

この記事では、AEMプロジェクトにサードパーティの OSGi バンドルを含める手順について説明します。この記事の目的で、 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) をAEMプロジェクトに追加します。  OSGi が maven リポジトリで使用可能な場合は、プロジェクトの POM.xml ファイルにバンドルの依存関係が含まれます。

>[!NOTE]
> サードパーティの jar が OSGi バンドルであると想定されます。

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

OSGi バンドルがファイルシステム上にある場合は、という名前のフォルダーを作成します。 **localjar** プロジェクトのベースディレクトリ (C:\aemformsbundles\AEMFormsProcessStep\localjar) の下で、依存関係は次のようになります

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

このバンドルをAEMプロジェクトに追加しています **AEMFormsProcessStep** これは **c:\aemformsbundles** フォルダー

* を開きます。 **filter.xml** C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter elementから。

* 次のフォルダー構造を作成しますC:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* この **apps/AEMFormsProcessStep-vendor-packages** は filter.xml のルート属性値です
* プロジェクトの POM.xml の依存関係セクションを更新します。
* コマンドプロンプトを開きます。私の場合は、プロジェクトのフォルダー (c:\aemformsbundles\AEMFormsProcessStep) に移動します。 次のコマンドを実行します。

```java
mvn clean install -pAutoInstallSinglePackage
```

問題が解決しない場合、パッケージはサードパーティバンドルと共にAEMインスタンスにインストールされます。 次を使用して、バンドルを確認できます。 [felix web コンソール](http://localhost:4502/system/console/bundles). サードパーティバンドルは、 `crx` 次に示すリポジトリ
![サードパーティ](assets/custom-bundle1.png)




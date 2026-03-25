---
title: AEM as a Cloud Serviceで非推奨のAPIを検索して削除する
description: AEM as a Cloud Serviceで非推奨のAPIを検索および削除する方法について説明します。
version: Experience Manager as a Cloud Service
role: Developer
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
exl-id: 287894ea-9cc1-4c27-ac7e-967ad46f4789
source-git-commit: 30b98e82e78120bf9fb13c9d41780af4c07665d8
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---

# AEM as a Cloud Serviceで非推奨のAPIを検索して削除する

AEM as a Cloud Serviceで非推奨のAPIを検索および削除する方法について説明します。

## 概要

アプリケーションが安全でパフォーマンスが高く、Cloud Manager パイプラインを使用してコードを引き続きデプロイできるようにするには、非推奨のAPIをプロジェクトから削除します。

このチュートリアルでは、[AEM Analyzer Maven プラグイン ](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)を使用して、AEM as a Cloud Service環境で非推奨のAPIを検索して削除する方法について説明します。

## 非推奨のAPIに関する通知

非推奨のAPIの使用状況と、それを修正するための注意が定期的に報告されます。いくつかの例を確認しましょう。

- AEM as a Cloud Service **アクション センター**&#x200B;は、プロジェクトで非推奨となったAPI _について通知します。_
  ![ アクション センターで非推奨のAPI](./assets/deprecated-apis/actions-center-deprecated-apis.png)

- Cloud Manager パイプラインの&#x200B;**コードスキャン**&#x200B;手順では、プロジェクトで非推奨のAPIがレポートされます。非推奨のAPIの完全なリストについては、**詳細をダウンロード**のレポートを参照してください。
  ![ コードスキャンで非推奨のAPI](./assets/deprecated-apis/code-scanning-summary.png)

- Cloud Manager パイプラインの&#x200B;**アーティファクト準備** ステップでは、プロジェクトで非推奨のAPIがレポートされます。**ログをダウンロード**&#x200B;し、ログファイルで&#x200B;_Analyzerの警告_&#x200B;を探します。

  ```
  2026-02-20 15:40:48.376 Analyser warnings have been found 
  2026-02-20 15:40:48.376 The analyser found the following warnings for author and publish : 
  2026-02-20 15:40:48.376 [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
  2026-02-20 15:40:56.458 Convert Merge Analyse finished.
  ```


## 非推奨のAPIの検索方法

AEM as a Cloud Service プロジェクトで非推奨のAPIを見つけるには、次の手順に従います。

1. **最新のAEM Analyzer Maven プラグインを使用**

   AEM プロジェクトで、[AEM Analyzer Maven プラグイン ](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)の最新バージョンを使用します。

   - メイン `pom.xml`では、通常、プラグインのバージョンが宣言されます。 お使いのバージョンと最新の[ リリース版](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin)を比較します。

     ```xml
     ...
     <aemanalyser.version>1.6.16</aemanalyser.version> <!-- Latest released version as of 20-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - プラグインは、利用可能な最新のAEM SDKと照合されます。 プロジェクトの`pom.xml` ファイルで最新のAEM SDK バージョンを使用します。 非推奨APIをIDE警告として表示するのに役立ちます。

     ```xml
     ...
     <aem.sdk.api>2026.2.24464.20260214T050318Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 20-Feb-2026 -->
     ...
     ```

   - `all` モジュールが`verify` フェーズでプラグインを実行していることを確認します。

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **ビルドを実行して警告を確認する**

   `mvn clean install`を実行すると、アナライザーは非推奨APIを&#x200B;**[警告]** メッセージとして出力します。 次に例を示します。

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   構築の成功や失敗に焦点を当てる場合、これらのメッセージを見落とすことがよくあります。

3. **非推奨のAPIの明確なリストを取得**

   上記の手順でも同じ情報が提供されます。 ただし、`verify` モジュールで`all` フェーズを実行して、すべての&#x200B;**[警告]** メッセージを1か所で表示します。 次に例を示します。

   ```shell
   $ mvn clean verify -pl all
   ```

   ビルド出力の&#x200B;**[WARNING]** メッセージは、プロジェクト内の非推奨APIの一覧です。

## 非推奨のAPIを削除する方法

AEM Analyzerは、**what**&#x200B;が非推奨であると報告し、その修正方法について&#x200B;**推奨事項**&#x200B;を提供します。 ただし、以下の表を使用して適切なアクションを選択し、詳細が必要な場合はリンクされたドキュメントに従います。

### 非推奨のAPI修復戦略

| アナライザーの警告タイプ | 本文書の内容 | 推奨されるアクション | 参照 |
| --------------------- | ----------------- | ------------------ | --------- |
| 非推奨のAEM API | APIをAEM as a Cloud Serviceから削除する | 使用をサポートされているパブリック APIに置き換える | [API削除ガイダンス ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 非推奨のAEM パッケージまたはクラス | パッケージまたはクラスはサポートされなくなりました | 推奨される代替方法を使用するには、コードをリファクタリングします | [非推奨のAPI](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| 非推奨のサードパーティライブラリ | ライブラリは、今後のSDKではサポートされません | 依存関係のアップグレードと使用状況のリファクタリング | [一般ガイドライン ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 非推奨のSling / OSGi パターン | レガシー注釈またはAPIが検出されました | 最新のSling APIおよびOSGi APIへの移行 | [Sling / OSGi パターンの削除](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 計画的な削除（先日付） | APIは引き続き機能しますが、削除は後で適用されます | パイプラインの適用前にクリーンアップをスケジュール | [リリースノート](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/release-notes/home) |

### 実践的ガイダンス

- アナライザーの警告は、オプションのメッセージではなく、**将来のパイプラインのエラー**&#x200B;として扱います。
- 非推奨のAPIをローカルで修正するには、**最新のAEM SDK**&#x200B;を使用します。
- 今後のAEMのアップグレード中に発生する問題を回避するために、アナライザー出力をクリーンに保ちます。

非推奨APIを早期に修正すると、プロジェクト **アップグレードの安全性とデプロイメントの準備が維持されます**。

## その他のリソース

- [AEM Analyzer Maven プラグイン ](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [非推奨および削除された機能とAPI](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

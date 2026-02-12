---
title: AEM as a Cloud Serviceでの非推奨 API の検索と削除
description: AEM as a Cloud Serviceで非推奨の API を検索して削除する方法を説明します。
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
exl-id: 287894ea-9cc1-4c27-ac7e-967ad46f4789
source-git-commit: effacd58725dab502f6fb6a4750646c1ea956de2
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 3%

---

# AEM as a Cloud Serviceでの非推奨 API の検索と削除

AEM as a Cloud Serviceで非推奨の API を検索して削除する方法を説明します。

## 概要

AEM as a Cloud Service **アクションセンター** は、プロジェクトの _非推奨の API_ について通知します。 アプリケーションのセキュリティとパフォーマンスを確保し、Cloud Manager パイプラインを使用してコードのデプロイを続行できるようにするには、プロジェクトから非推奨（廃止予定）の API を削除します。

このチュートリアルでは、[AEM Analyzer Maven プラグイン &#x200B;](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md) を使用して、AEM as a Cloud Service環境で非推奨の API を検索および削除する方法について説明します。

## 非推奨 API を検索する方法

AEM as a Cloud Service プロジェクトで非推奨（廃止予定）の API を検索するには、次の手順に従います。

1. **最新のAEM Analyzer Maven プラグインを使用する**

   AEM プロジェクトでは、[AEM Analyzer Maven プラグインの最新バージョンを使用し &#x200B;](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md) す。

   - メイン `pom.xml` ージでは、通常、プラグインのバージョンが宣言されます。 お使いのバージョンを最新の [&#x200B; リリースバージョン &#x200B;](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin) と比較します。

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
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

   - このプラグインは、使用可能な最新のAEM SDKに対してチェックします。 プロジェクトの `pom.xml` ファイルで最新バージョンのAEM SDKを使用します。 非推奨（廃止予定）の API を IDE の警告として表示するのに役立ちます。

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - `all` モジュールが `verify` フェーズでプラグインを実行することを確認します。

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

   `mvn clean install` を実行すると、アナライザーは、非推奨 API を **[警告]** メッセージとして出力に報告します。 例：

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   ビルドの成功または失敗に焦点を当てる場合、これらのメッセージを見過ごすのは簡単です。

3. **非推奨（廃止予定）の API の明確なリストの取得**

   上記の手順でも同じ情報が提供されます。 ただし、`verify` モジュールで `all` フェーズを実行すると、すべての **[警告]** メッセージが 1 か所で表示されます。 例：

   ```shell
   $ mvn clean verify -pl all
   ```

   ビルド出力の **[警告]** メッセージには、プロジェクト内の非推奨（廃止予定）の API が一覧表示されます。

## 非推奨 API を削除する方法

AEM Analyzer は、非推奨となった **対象** をレポートし、その修正方法に関する **推奨事項** を提供します。 ただし、以下の表を使用して適切なアクションを選択し、詳細が必要な場合はリンク先のドキュメントに従ってください。

### 非推奨（廃止予定）の API 修復戦略

| アナライザー警告タイプ | 説明 | 推奨されるアクション | 参照 |
| --------------------- | ----------------- | ------------------ | --------- |
| 非推奨（廃止予定）のAEM API | API はAEM as a Cloud Serviceから削除されます | 使用をサポートされているパブリック API に置き換えます。 | [API 削除ガイダンス &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 非推奨のAEM パッケージまたはクラス | パッケージまたはクラスはサポートされなくなりました | コードをリファクタリングして、推奨される代替値を使用します | [&#x200B; 非推奨の API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| 非推奨のサードパーティライブラリ | ライブラリは今後の SDK ではサポートされません | 依存関係のアップグレードとリファクタリングの使用 | [&#x200B; 一般指針 &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 非推奨の Sling/OSGi パターン | 従来の注釈または API が検出されました | 最新の Sling API および OSGi API への移行 | [Sling/OSGi パターンの削除 &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 削除予定（将来の日付） | API は引き続き機能しますが、後で削除が適用されます | パイプラインの適用前にクリーンアップをスケジュール | [リリースノート](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/release-notes/home) |

### 実践指導

- アナライザーの警告を、オプションのメッセージではなく、**今後のパイプラインの失敗** として扱う。
- **最新のAEM SDK** を使用して、非推奨（廃止予定）の API をローカルで修正しました。
- 今後のAEMのアップグレード中に発生する問題を回避するために、アナライザーの出力をクリーンに保ちます。

非推奨 API を早期に修正することで、プロジェクトを **アップグレードしても安全で、デプロイメントに対応** できます。

## その他のリソース

- [AEM Analyzer Maven プラグイン &#x200B;](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [&#x200B; 非推奨（廃止予定）の機能と削除された機能および API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

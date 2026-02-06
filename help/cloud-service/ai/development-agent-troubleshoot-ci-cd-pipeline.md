---
title: AEM Development Agent を使用した CI/CD Pipeline のトラブルシューティング
description: AEM Development Agent を使用して、失敗した CI/CD パイプラインのトラブルシューティングと修正方法について説明します。
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 3%

---


# AEM Development Agent を使用した CI/CD Pipeline のトラブルシューティング

AEM Development Agent を使用して、失敗した CI/CD パイプラインのトラブルシューティングと修正方法について説明します。

AEM Development Agent は、開発者、DevOps エンジニア、管理者などの技術チームが **AI を活用したガイダンスとアクション** を提供することで、ワークフローを _高速化_ するのに役立ちます。

>[!TIP]
>
> AEMで使用可能なエージェントの完全なリスト、その機能およびアクセス方法については、[AEM as a Cloud Serviceのエージェントの概要 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) も参照してください。


## 概要

AEM Development Agent には、失敗した CI/CD パイプラインの一覧表示、トラブルシューティング、修正機能を含む、複数の機能があります。 AI アシスタントを通じてAEM Development Agent を呼び出し、特定の使用例に対処することができます。

このチュートリアルでは [WKND Sites プロジェクト ](https://github.com/adobe/aem-guides-wknd) を使用して、AEM Development Agent で失敗した CI/CD パイプラインのトラブルシューティングおよび修正方法を示します。 同じ原則が、あらゆるAEM プロジェクトに当てはまります。

簡単にするために、このチュートリアルでは、AEM Development Agent のパイプライントラブルシューティング機能を紹介する単体テストのエラーを `BylineImpl.java` ファイルで紹介します。

## 前提条件

このチュートリアルに従うには、以下が必要です。

- AEMの AI アシスタントとエージェントを有効にしました。 詳しくは、[AEMでの AI の設定 ](./setup.md) を参照してください。また、この記事に記載されているプレイグラウンドには、AEM Development Agent の機能がないことに注意してください。
- 開発者またはプログラムマネージャーの役割でAdobe [0}Cloud Manager} にアクセスします。 ](https://my.cloudmanager.adobe.com/)詳しくは、[ 役割の定義 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions) を参照してください。
- AEM as a Cloud Service環境
- [Beta プログラム ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs) を介してAEMのエージェントにアクセスします
- ローカルマシンに複製された [WKND Sites プロジェクト ](https://github.com/adobe/aem-guides-wknd)

### AEM Development Agent の現在の機能

このチュートリアルに入る前に、AEM Development Agent の現在の機能を確認してください。

- CI/CD パイプラインとそのステータスの一覧表示
- **コード品質** と _デプロイメント_ の両方のタイプを含む、失敗した _フルスタック_ パイプラインのトラブルシューティングと修正。
- _フルスタック_ パイプラインの _ビルド_ （コードをコンパイルしてデプロイ可能なアーティファクトを作成）手順と **コード品質** （SonarQube ルールを介した静的コード分析）手順がサポートされます。

AEM Development Agent の機能は、定期的に拡張および更新されます。 フィードバックおよびご提案については、[aem-devagent@adobe.com](mailto:aem-devagent@adobe.com) までお問い合わせください。

## セットアップ

このチュートリアルを完了するには、次の大まかな手順に従います。

1. [WKND Sites プロジェクト ](https://github.com/adobe/aem-guides-wknd) を複製し、Cloud Manager Git リポジトリにプッシュします
2. コード品質パイプラインの作成と設定
3. パイプラインを実行し、失敗した実行を確認します
4. AEM Development Agent を使用して、失敗したパイプラインのトラブルシューティングと修正を行います

各手順を詳しく説明します。

### WKND Sites プロジェクトをデモプロジェクトとして使用

このチュートリアルでは、WKND Sites プロジェクトの `tutorial/dev-agent/unit-test-failure` ブランチを使用して、AEM Development Agent の使用方法を示します。 同じ原則を任意のAEM プロジェクトに適用できます。

- `BylineImpl.java` ファイルに次のような単体テストの失敗が導入されました。 独自のAEM プロジェクトを使用している場合、同様の単体テストのエラーが発生する可能性があります。

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- [WKND Sites プロジェクト ](https://github.com/adobe/aem-guides-wknd) をローカルマシンに複製し、プロジェクトディレクトリに移動して、`tutorial/dev-agent/unit-test-failure` ブランチに切り替えます。

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- WKND サイトプロジェクト用の新しいCloud Manager Git リポジトリを作成し、リモートとしてローカル Git リポジトリに追加します。

   - Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) に移動し、プログラムを選択します。
   - 左側のサイドバーで **リポジトリ** をクリックします。
   - 右上隅の **リポジトリを追加** をクリックします。
   - **リポジトリ名** を入力し（「wknd-site-tutorial」など）、「**保存**」をクリックします。 リポジトリが作成されるのを待ちます。

     ![ リポジトリを追加 ](./assets/dev-agent/add-repository.png)

   - 右上隅の **リポジトリ情報にアクセス** をクリックし、リポジトリ URL をコピーします。

     ![ リポジトリ情報にアクセス ](./assets/dev-agent/access-repo-info.png)

   - 新しく作成したCloud Manager Git リポジトリを、リモートとしてローカル Git リポジトリに追加します。

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- ローカル Git リポジトリをCloud Manager Git リポジトリにプッシュします。

  ```shell
  git push adobe
  ```

  資格情報の入力を求められたら、**ユーザー名**&#x200B;および&#x200B;**パスワード**&#x200B;を Cloud Manager の&#x200B;**リポジトリ情報**&#x200B;モーダルから指定します。

### コード品質パイプラインの作成と設定

このチュートリアルでは、コード品質パイプライン（実稼動以外）を使用して、トラブルシューティング用にパイプラインのエラーをトリガーします。 コード品質パイプラインについて詳しくは、[CI/CD パイプラインの概要 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction) を参照してください。

- Cloud Managerで、「**パイプライン**」セクションに移動して、**追加**/**実稼動以外のパイプラインを追加** を選択します。
- **実稼動以外のパイプラインを追加** ダイアログで、以下を設定します。

   - **設定** 手順：
      - 「**パイプラインタイプ**」などのデフォルト値は `Code Quality Pipeline` として、「**デプロイメントトリガー** を `Manual` として維持します。
      - 「**実稼動以外のパイプライン名**」に `Code Quality::Fullstack` と入力します。

     ![ 実稼動以外のパイプライン設定を追加 ](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - **Source コード** 手順：
      - 「**フルスタックコード**」を選択します。
      - **リポジトリ** については、新しく作成したCloud Manager Git リポジトリを選択します。
      - **Git ブランチ** の場合は、「`tutorial/dev-agent/unit-test-failure`」を選択します
      - 「**保存**」をクリックします。

     ![ 実稼動以外のパイプラインのSource コードを追加 ](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- パイプラインエントリの 3 ドットメニューにある「**実行** をクリックして、新しく作成したコード品質パイプラインを実行します。

  ![ コード品質パイプラインの実行 ](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> デプロイメントパイプラインについては、このチュートリアルでは扱いません。 ただし、同じ原則に従って、失敗したデプロイメントパイプラインのトラブルシューティングと修正を行うことができます。


### 失敗したパイプラインの実行を確認します

**アーティファクトの準備** ステップでコード品質パイプラインが失敗し、次のエラーが表示されます。

![ 失敗したパイプラインの実行 ](./assets/dev-agent/failed-pipeline-execution.png)

AEM Development Agent がない場合、このパイプラインのエラーは手動のトラブルシューティングが必要です。 開発者は、ログを確認し、コードを確認する必要があります。これは面倒で時間のかかるプロセスです。

次に、失敗したパイプライン実行のトラブルシューティングと修正を Agentic AI で行う方法を確認します。

## AEM Development Agent を使用して、失敗したパイプラインのトラブルシューティングと修正をおこないます

AEMで AI アシスタントを使用してAEM Development Agent を呼び出すことができます。そのためには、パイプラインのエラーを自然言語で記述します。

- 右上隅にある **AI アシスタント** アイコンをクリックします。

- パイプライン失敗の詳細を、自然言語 **プロンプト** で入力します。 例：

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![AEM Development Agent の呼び出し ](./assets/dev-agent/invoke-aem-development-agent.png)

  **AEM開発エージェント** が呼び出され、失敗したパイプラインの実行のトラブルシューティングと修正が行われます。

  >[!NOTE]
  >
  > 入力したプロンプトが明確でない場合は、AI アシスタントが明確な説明を求め、プロンプトを絞り込むのに役立つ情報を提供します。

- 推論が完了したら、「**フルスクリーンで開く** アイコンをクリックして、詳細なトラブルシューティングプロセスを表示します。

  ![ 全画面表示で開く ](./assets/dev-agent/open-in-full-screen.png)

  結果には、エラーの詳細、ソースファイル、行番号、問題を解決する明確な手順を示す **修正方法** セクションなど、貴重なインサイトが含まれています。

- この場合、エージェントは、問題を修正するために、実装を変更する（`getName()` の方法）か、単体テストを更新する（`getNameTest()` の方法）かを正しく提案しました。 幻覚を避け、開発者に実用的なコード変更を提供しながら、Human-in-the-Loop アプローチを使用しました。

  ![ コードの変更をコピー ](./assets/dev-agent/copy-code-changes.png)

- `BylineImpl.java` ファイルを推奨されるコード変更で更新し、変更内容をコミットしてCloud Manager Git リポジトリにプッシュします。

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- パイプラインを再度実行し、正常に実行されたことを確認します。

## その他の例

WKND Sites プロジェクトには、依存関係の欠如や間違った設定など、コードの破損や設定の問題の例がさらに含まれています。 これらの例は、[`tutorial/dev-agent/` で始まる ](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview)branch をチェックアウトすることで確認できます。 重大な変更を確認するには、「`tutorial/dev-agent/unit-test-failure` 比較 `main` ボタンをクリックして、**ブランチと** ブランチを比較します。 次に、_変更されたファイル_ セクションを探します。

![ ブランチを比較 ](./assets/dev-agent/compare-branches.png)

AEM Development Agent の使用方法について詳しくは、[ サンプルプロンプト ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts) も参照してください。

## 概要

このチュートリアルでは、AEM Development Agent を使用して、AI アシスタントで失敗した CI/CD パイプラインのトラブルシューティングと修正を行う方法を学びました。 また、Agentic AI が、実用的なインサイトとコード変更を提供することで、テクニカルワークフローを加速させる方法についても学びました。

AEM Development Agent およびAEMのその他のエージェントを使用してワークフローを高速化します。詳しくは、[AEMのエージェントの概要 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) を参照してください。

## その他のリソース

- [Experience Managerの AI](./overview.md)
- [AEMにおけるエージェントの概要 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [ 開発エージェントの概要 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [AEMにおけるエージェントの概要 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)

---
title: 「DDX を呼び出し」操作を使用した Forms CS での PDF 操作
description: 「DDX を呼び出し」を使用した PDF ファイルのアセンブリ。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '102'
ht-degree: 100%

---

# はじめに

このコースでは、Forms CS を使用した PDF ドキュメントの PDF 操作とアーカイブを使用します。外部アプリケーションからこれらのマイクロサービスを使用するには、次の手順に従います。

1. AEM テクニカルアカウントのサービス資格情報を生成します。
1. サービス資格情報から JSON web トークン（JWT）を作成し、アクセストークンと交換します。
1. AEM でテクニカルアカウントのアクセスを設定します。
1. アクセストークンを使用して HTTP 呼び出しを行い、PDF ファイルを操作したり、PDF/A ファイルを生成し検証したりします

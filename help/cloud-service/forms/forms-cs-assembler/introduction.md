---
title: 「invoke DDX」操作を使用したForms CS でのPDF操作
description: invoke DDX を使用してPDFファイルをアセンブリします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
duration: 19
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 48%

---

# はじめに

このコースでは、Forms CS を使用したPDFドキュメントのPDF操作とアーカイブを使用します。 外部アプリケーションからこれらのマイクロサービスを使用するには、次の手順に従います。

1. AEM テクニカルアカウントのサービス資格情報を生成します。
1. サービス資格情報から JSON web トークン（JWT）を作成し、アクセストークンと交換します。
1. AEM でテクニカルアカウントのアクセスを設定します。
1. アクセストークンを使用して HTTP 呼び出しを行い、PDFファイルを操作し、PDF/A ファイルを生成して検証します

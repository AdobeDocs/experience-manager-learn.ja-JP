---
title: 「invoke DDX」操作を使用したForms CS でのPDF操作
description: invoke DDX を使用してPDFファイルをアセンブリします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# はじめに

このコースでは、Forms CS を使用したPDFドキュメントのPDF操作とアーカイブを使用します。 外部アプリケーションからこれらのマイクロサービスを使用するには、次の手順に従います。

1. AEMテクニカルアカウントのサービス資格情報を生成します
1. サービス資格情報から JSON Web トークン (JWT) を作成し、アクセストークンと交換します
1. AEMのテクニカルアカウントへのアクセスの設定
1. アクセストークンを使用して HTTP 呼び出しを行い、PDFファイルを操作し、PDF/A ファイルを生成して検証します

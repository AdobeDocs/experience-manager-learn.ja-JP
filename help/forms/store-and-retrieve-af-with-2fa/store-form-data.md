---
title: フォームデータの保存
description: 新しい添付ファイルマップと共にフォームデータをデータベースに格納します。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6538
thumbnail: 6538.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 2bd9fe63-8f42-4b89-95a0-13ade49bc31b
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '75'
ht-degree: 100%

---

# フォームデータの保存

次に、アダプティブフォームのデータおよび関連する attachmentsinfo を格納する新しい行をデータベースに挿入するサービスを作成します。
次のスクリーンショットは、データベース内の行を示しています。


![サンプル行](assets/sample-row.JPG)


次のコードでは、適切なデータを含んだ新しい行をデータベースに挿入しています。

```java
public String storeFormData(String formData, String attachmentsInfo, String telephoneNumber) {
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData + "and the telephone number to insert is  " + telephoneNumber);
    String insertRowSQL = "INSERT INTO aemformstutorial.formdatawithattachments(guid,afdata,attachmentsInfo,telephoneNumber) VALUES(?,?,?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
        pstmt = null;
        pstmt = c.prepareStatement(insertRowSQL);
        pstmt.setString(1, randomUUIDString);
        pstmt.setString(2, formData);
        pstmt.setString(3, attachmentsInfo);
        pstmt.setString(4, telephoneNumber);
        log.debug("Executing the insert statment  " + pstmt.executeUpdate());
        c.commit();
    } catch (SQLException e) {

        log.error("unable to insert data in the table", e.getMessage());
    } finally {
        if (pstmt != null) {
            try {
                pstmt.close();
            } catch (SQLException e) {
                log.debug("error in closing prepared statement " + e.getMessage());
            }
        }
        if (c != null) {
            try {
                c.close();
            } catch (SQLException e) {
                log.debug("error in closing connection " + e.getMessage());
            }
        }
    }
    return randomUUIDString;
}
```

## 次の手順

[保存と終了の実装](./create-servlet.md)


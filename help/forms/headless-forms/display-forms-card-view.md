---
title: 取得したフォームをカード表示で表示
description: フォームを表示するには、listforms API を使用します
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
exl-id: 7316ca02-be57-4ecf-b162-43a736b992b3
source-git-commit: 529e98269a08431152686202a8a2890712b9c835
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# フォームをカード形式で取得して表示する

カード表示形式は、情報やデータをカード形式で表示するデザインパターンです。 各カードは、個別のコンテンツまたはデータ入力を表し、通常は、特定の要素が配置された視覚的に異なるコンテナで構成されます。
React のクリック可能なカードは、カードやタイルに似たインタラクティブコンポーネントで、ユーザーがクリックまたはタップできます。 クリック可能なカードをクリックまたはタップすると、別のページへの移動、モーダルの開く、UI の更新など、指定したアクションまたは動作がトリガーされます。

この記事では、 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) フォームを取得してカード形式でフォームを表示するには、click イベントでアダプティブフォームを開きます。

![カード表示](./assets/card-view-forms.png)

## カードテンプレート

次のコードは、カードテンプレートのデザインに使用されました。 カードテンプレートには、アダプティブフォームのタイトルと説明がAdobeロゴと共に表示されます。 [マテリアル UI コンポーネント](https://mui.com/) は、このレイアウトの作成に使用されています。



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
   
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.id}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

次のルートが Main.js で定義され、DisplayForm.js に移動します。

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## フォームを取得

listforms API は、AEMサーバーからフォームを取得するために使用されました。 API は JSON オブジェクトの配列を返し、各 JSON オブジェクトはフォームを表します。

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

上記のコードでは、 map 関数を使用して fetchedForms を繰り返し処理し、 fetchedForms 配列内の各アイテムに対して FormCard コンポーネントを作成し、 Grid コンテナに追加します。 これで、必要に応じて、React アプリで ListForm コンポーネントを使用できます。

## 次の手順

[ユーザーがカードをクリックしたときにアダプティブフォームを表示する](./open-form-card-view.md)
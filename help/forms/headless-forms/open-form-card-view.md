---
title: カードのクリックによるフォームの表示
description: カード表示からフォームにドリルダウンします
feature: Adaptive Forms
version: 6.5
jira: KT-13372
topic: Development
role: User
level: Intermediate
exl-id: c8684cd9-b9c5-4b5b-b990-27c5700cea9f
duration: 25
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '61'
ht-degree: 100%

---

# カードクリック時のフォームの表示

ユーザーがカードをクリックしたときにフォームを表示するために、次のコードを使用しました。表示するフォームのパスは、useParams 関数を使用して URL から抽出されます。

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`/adobe/forms/af/${params.formID}`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```

## 次の手順

[フォーム送信時に「ありがとうございます」メッセージを表示](./display-thank-you-message.md)

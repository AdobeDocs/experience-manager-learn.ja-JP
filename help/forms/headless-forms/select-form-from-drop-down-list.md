---
title: 使用可能なフォームリストからのフォームの選択
description: listforms API を使用してドロップダウンリストにデータを入力します
feature: Adaptive Forms
version: 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 100%

---

# ドロップダウンリストからの入力対象フォームの選択

ドロップダウンリストを使用すると、オプションのリストを整理された形でコンパクトにユーザーに提示でできます。ドロップダウンリスト内の項目には、[listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) の結果が入力されます。

![カード表示](./assets/forms-drop-down.png)

## ドロップダウンリスト

listforms API 呼び出しの結果をドロップダウンリストに入力するために、次のコードを使用しました。ユーザーの選択に応じて、ユーザーが入力して送信するためのアダプティブフォームが表示されます。このインターフェイスの作成には、[Material UI コンポーネント](https://mui.com/)を使用しています。

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

このユーザーインターフェイスの作成では、次の 2 つの API 呼び出しを使用しました。

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)。フォームを取得する呼び出しは、コンポーネントのレンダリング時に 1 回だけ行われます。API 呼び出しの結果は、afForms 変数に格納されます。
上記のコードでは、map 関数を使用して afForms を反復処理し、afForms 配列内の項目ごとに MenuItem コンポーネントを作成して Select コンポーネントに追加します。

* フォームの取得 - [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition) に対して GET 呼び出しが行われます。id は、ドロップダウンリストでユーザーが選択したアダプティブフォームの ID です。この GET 呼び出しの結果は、selectedForm に格納されます。

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* 選択されたフォームの表示。選択されたフォームを表示するために、次のコードを使用しました。AdaptiveForm 要素は aemforms/af-react-renderer npm パッケージに用意されており、mappings と formJson がその要素のプロパティとして必要です。

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## 次の手順

[カードレイアウトでのフォームの表示](./display-forms-card-view.md)

# Composition blocks

Há três formas de enviar um bloco declarado e estilizado no storefront (JSON) para um componente custom (React), uma delas é por meio do **composition blocks**. Com ele podemos enviar uma referencia de um bloco para um app custom e chamalo uma ou mais vezes. Além disso, diferente do **composition children**, é possível enviar propiedades dentro do componente para esse bloco.

Como enviar o bloco pelo storefront:

```jsonc
// exemplo: storefront > home.jsonc
{
    "my-custom-component": {
        "props": {
            "collectionId": "123"
        },
        "blocks": ["list-context.product-list#m3-shelf"] // envia um bloco do json para o componete custom
    },
    "list-context.product-list#m3-shelf": {
        "blocks": ["product-summary.shelf#m3-shelF"],
        "children": ["slider-layout#m3-shelf"]
    }
}
```

Configurando o bloco custom dentro do interfaces para receber o bloco do storefront:

```jsonc
// custom > interfaces.json
{
    "my-custom-component": {
        "component": "MyCustomComponent",
        // o APP que contém esse bloco deve estar declarado como dependencia no manifest.json do APP custom que esta criando
        // nesse exemplo o bloco "list-context.product-list" é do APP "vtex.product-summary"
        "allowed": ["list-context.product-list"] 
    }
}
```

Como chamar o bloco dentro do componente custom, nesse exemplo enviando a propiedade "collection": 

```tsx
import React from "react";

import { Block } from "vtex.render-runtime";

type MyCustomComponentProps = { collectionId: string }

function MyCustomComponent({ collectionId = "" }: MyCustomComponentProps) {
    return <Block id="list-context.product-list" collection={collectionId} />;
}

export default MyCustomComponent;
```

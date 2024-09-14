# Kobe Challange

Em um e-commerce, é muito comum que em um mesmo pedido, existam diferentes formas de entrega e/ou retirada para os produtos que estão sendo comprados. Essas formas de logística são chamadas de SLAs (service level agreement) e é como um "contrato" entre a empresa e o seu cliente que vai garantir que este receba a mercadoria pela qual pagou. A sua tarefa é agrupar os produtos em cada tipo disponível de SLA, para que o usuário tenha visibilidade das possibilidades de entrega/retirada dentro dos produtos que está comprando.

### Opções disponíveis de SLAs:

- Entrega Agendada
- Entrega Correios
- Retirada em Loja


#### Exemplo de entrada:
```json
[
  {
    "id": "1234",
    "name": "Camisa polo",
    "slas": ["Entrega Agendada", "Entrega Correios"]
  },
  {
    "id": "567",
    "name": "Calça jeans",
    "slas": ["Entrega Correios", "Retirada em loja"]
  },
  {
    "id": "890",
    "name": "Vestido",
    "slas": ["Retirada em loja"]
  }
]
```

#### Saída esperada:
```json
{
  "Entrega Agendada": [
    "Camisa Polo"
  ],
  "Entrega Correios": [
    "Camisa Polo",
    "Calça jeans"
  ],
  "Retirada em loja": [
    "Calça jeans",
    "Vestido"
  ]
}
```


### Quick and dirty solution

1. Define my input:

```js
const data = [
  {
    "id": "1234",
    "name": "Camisa polo",
    "slas": ["Entrega Agendada", "Entrega Correios"]
  },
  {
    "id": "567",
    "name": "Calça jeans",
    "slas": ["Entrega Correios", "Retirada em loja"]
  },
  {
    "id": "890",
    "name": "Vestido",
    "slas": ["Retirada em loja"]
  }
];
```

2. Define an sla keys array with values that will serve to: (I) search for matching slas on my input data and then (II) to be used as object keys in output:
```js
const slaKeys = ["Entrega Agendada", "Entrega Correios", "Retirada em Loja"];
```

3. You can use different approaches to solve this. I will use reduce() to create my output object based on sla keys, searching each input item on data, if it matches the sla name, then adds that to the current sla array:
```js
const result = slaKeys.reduce((acc, sla) => {
    acc[sla] = [];
    data.forEach(({ name, slas })=> {
        const exists = slas.find(slaName => slaName.toLowerCase() === sla.toLowerCase());
        if(exists) acc[sla].push(name);
    });
    return acc;
}, {});

```

You can test this on browser console :)

![image](https://github.com/user-attachments/assets/367e322e-ed31-4a1d-8c58-46bfea203b78)



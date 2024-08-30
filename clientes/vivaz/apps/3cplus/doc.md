# DOC 3CPLUS - VIVAZ VENDAS


## LEIA ANTES DE IMPLEMENTAR
O 3CPLUS tem a opção de mostrar dados sobre o lead para o agente durante a ligação.
- Atualmente isso é feito por meio de integração via api, para prosseguir veja as [INTEGRAÇÕES disponíveis.](#integrações)

## INTEGRAÇÕES
- [RD STATION](#rd-station)
- [PERSONALIZADO](#personalizado)

### LEADS

URL padrão para adicionar leads a uma listas
```
https://1v1connect.com/api/clients/vivaz/3cplus/leads.php
```

#### Method:
```
POST
```


#### HEADERS
```headers
Content-type: application-json
```

### RD STATION
Correspondência de dados para exibir durante ligação no 3cplus
```PHP
$dados = [
    "nome" => $request->data["name"],
    "email" => $request->data["email"],
    "telefone" => $request->data["telefone"], // Campo criado para guardar o telefone formatado (mobile_phone, personal_phone)
    "empreendimento" => $request->data["last_conversion"]["content"]["identificador"]
];

```

### PERSONALIZADO
Controle personalizado de dados para exibir durante ligação no 3cplus.
```JSON
{
    "nome": "Marcus Gomes", // Obrigatório
    "telefone": "5548991333814", // Obrigatório
    "origin": "rrr", // Se não for informado aqui, é obrigatorio informa por meio de query
    "email": "ti@wavemaker.com.br",
    "produto": "xpto",
    "renda": "R$ 1500000",
    "participantes": "50",
    "subsídio": "R$ 500000",
    "taxa": "5%",
    "valor_imovel": "R$ 150000000",
    "deseja_visitar": "sim",
    "data_visita": "30/08/24 as 14h30"
}
```

### QUERYS
- origin (OBRIGATÓRIO CASO NÃO SEJA O MÉTODO DE ENVIO PERSONALIADO)
- list_id (opcional)
- campaign_id (opcional)
- observacoes (opcional)

#### Caso os dados sejam de origem do RD Station
```PHP
// Concatenar observações + last_conversion
$request->data["observacoes"] .= $dados_rdstation["last_conversion"]["content"]["identificador"];
```

#### Caso ``list_id`` e ``campaign_id`` não forem informados, seguirá configuração da lista e campanha padrão
```PHP
// Define Lista
$list_id = $request->data["list_id"] ?? 1712696; // Lista padrão: Leads

// Define Campanha
$campaign_id = $request->data["campaign_id"] ?? 135044; // Campanha padrão: SDR - Novos Leads (Wavemaker)
```

# Origem: Notifica Lead
## Dispara webhook com os seguinte dados:
```JSON
{
  "created": "2024-07-09 18:00:42 -0300",
  "event_name": "new_lead",
  "user_id": "81",
  "lead": {
    "name": "Beltrano da silva",
    "phone": "5547999887766",
    "email": "email@teste.com",
    "obs": ""
  },
  "midia": {
    "channel": "webhook",
    "page": "",
    "property_id": "",
    "url": ""
  },
  "broker": {
    "broker_name": "Fulano de tal",
    "broker_whatsapp": "47999887766",
    "broker_cpf": "12345678900"
  }
}
```
# Destino: 1v1 Connect
## Recebe dos dados e faz as seguinte verificações:

- Se ``novo_deal=sim`` existir na url, força ação de **criar** novo deal no Sellit.
- Se `novo_deal=sim`` não existir na url, então procura o deal no sellit, se encontrar atualiza todos os deals, caso não encontre cria um novo deal.
- Se ``broker_cpf`` estiver preenchido será considerado como ``cpfProprietario`` para criar ou atualizar um deal.
- Se ``broker_cpf`` não tiver dados, verifica se ``broker_whatsapp`` possue dados e verifica o cpf do corretor no DB usando como chave de busca o ``broker_whatsapp``
- Se ``token_rd`` existe na query da url pega o token informado e grava conversão no RD Station.
- Se ``observacoes`` existir na query da url, será considerado como ``observacoes``, caso contrário consdiera ``obs`` em ``lead.obs`` no JSON NL.
- Se ``origin`` existir na query da url, será adicionado ao ``DB log``

### FLUXO DE AÇÕES:

## ``Dados NL``->``1v1 Connect``->``Cria/Atualiza Sellit``->``DB log``->Conversion RD Station

#### EXEMPLO URL COMPLETA
- token_rd
- observacoes
- novo_deal
- ogirin

``` 
https://1v1connect.com/api/clients/vivaz/sellit/index.php?observacoes=origem_tiktok&token_rd=3a2c488e22aeb2c23c41197b40962cef&novo_deal=sim&origin=testeorr
```
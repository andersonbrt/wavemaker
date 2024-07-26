# DOCUMENTAÇÃO DE SERVIÇOS API - WAVEMAKER

## Menu de integrações

- [Notifica Lead -> Sellit](#nl-sellit)
- [RD -> Sellit](#rd-sellit)
- [Custom -> Sellit](#custom-sellit)

### Documentação com informações de regra de negócio de acordo com cada tipo de integração, pode conter algums termos técnicos, porém sem grau de complexidade.


# ``NL->SELLIT``
## Origem: Notifica Lead
#### Dispara webhook com os seguinte dados:
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
## Destino: 1v1 Connect
#### Recebe dos dados e faz as seguinte verificações:

- Se ``novo_deal=sim`` existir na url, força ação de **criar** novo deal no Sellit.
- Se `novo_deal=sim`` não existir na url, então procura o deal no sellit, se encontrar atualiza todos os deals, caso não encontre cria um novo deal.
- Se ``broker_cpf`` estiver preenchido será considerado como ``cpfProprietario`` para criar ou atualizar um deal.
- Se ``broker_cpf`` não tiver dados, verifica se ``broker_whatsapp`` possue dados e verifica o cpf do corretor no DB usando como chave de busca o ``broker_whatsapp``
- Se ``token_rd`` existe na query da url pega o token informado e grava conversão no RD Station.
- Se ``observacoes`` existir na query da url, será considerado como ``observacoes``, caso contrário consdiera ``obs`` em ``lead.obs`` no JSON NL.
- Se ``origin`` existir na query da url, será adicionado ao ``DB log``

## FLUXO DE AÇÕES:
## ``Dados NL``->``1v1 Connect``->``Cria/Atualiza Sellit``->``DB log``->Conversion RD Station
#### EXEMPLO URL COMPLETA

``` 
https://1v1connect.com/api/clients/vivaz/sellit/index.php?observacoes=origem_tiktok&token_rd=3a2c488e22aeb2c23c41197b40962cef&novo_deal=sim&origin=testeorr
```

#### Querys
- token_rd
- observacoes
- novo_deal
- ogirin

# ```RD`->SELLIT``
## Origem: RD Station
#### Dispara webhook com os seguinte dados:
```JSON
{
    "data": {
        "info": {
            "Id365": "a28f8b71-7c2d-ef11-8e4f-6045bd3b1e9b",
            "DataCriacao": "2024-06-18T14:08:58Z",
            "DataHoraModificacaoLead": "2024-06-18T14:08:58Z",
            "IdLead365": "908f8b71-7c2d-ef11-8e4f-6045bd3b1e9b",
            "CodigoLead": "F4092866",
            "IdRegional365": "bdec69b1-5d2f-e311-b421-02bfc0a801bb",
            "IdBandeira365": "8b96ed5a-43d9-e711-80c0-005056955165",
            "IdCorretor365": "77b9fc52-6818-ef11-840a-002248df229b",
            "NomeCorretor": "Luiz Carlos Reis Vieira",
            "EmailCorretor": "luiz.vieira@vivazvendas.com.br",
            "TelefoneCorretor": "",
            "IdCorretorOrigem365": "",
            "NomeCorretorOrigem": "",
            "PossuiCompartilhamento": "false",
            "IdEtapaFunil365": "50e83435-e232-ed11-9db2-002248370cfb",
            "NomeEtapaFunil": "Novo Lead",
            "IdMotivoDesqualificacaoLead365": "",
            "LeadDesqualificado": "false",
            "IdLeadCrm2013": "07411b71-7c2d-ef11-80d2-00155d7c350a",
            "CodigoLeadCrm2013": "F4092866",
            "DataCriacaoLeadCrm2013": "",
            "DataHoraUltimaAtualizacaoLeadCrm2013": "",
            "IdUsuarioProprietarioLeadCrm2013": "",
            "Nome": "Luiz Fernando Gomes Santos",
            "Telefone1": "5511982860663",
            "Telefone2": "",
            "Telefone3": "",
            "CodigoTelefonePrincipal": "1",
            "TelefoneOrigem": "telefone1",
            "Email1": "11982860663@nãotememailoferta.com",
            "Email2": "",
            "Email3": "",
            "CodigoEmailPrincipal": "1",
            "EmailOrigem": "email1",
            "Observacoes": "Prospectado por Oferta ativa",
            "IdTemperaturaLead365": "f118fc56-e132-ed11-9db2-00224837a176",
            "NomeTemperatura": "Morno",
            "CodigoTemperatura": "1",
            "CodigoFarolLead": "",
            "IdOrigemLead365": "",
            "NomeOrigemLead": "",
            "IdCanalContato365": "948b5bd3-af2a-ed11-9db2-000d3a88da5b",
            "NomeCanalContato": "Oferta Ativa - Psysx",
            "IdEmpreendimentoOrigem365": "",
            "NomeEmpreendimentoOrigem": "",
            "NomeEmpreendimentoOrigem2": "",
            "NomeEmpreendimentoOrigem365": "",
            "IdZonaEmpreendimento365": "",
            "NomeZonaEmpreendimento365": "",
            "ZonasEmpreendimentos": "",
            "DataNascimento": "",
            "IdSexo365": "",
            "Sexo": "",
            "IdProfissao365": "",
            "Profissao": "",
            "Cpf": "",
            "IdEstadoCivil365": "",
            "NomeEstadoCivil": "",
            "IdRendaFamiliar365": "",
            "RendaFamiliar": "",
            "Cep": "",
            "Bairro": "",
            "Cidade": "",
            "BairroComercial": "",
            "IdBairroComercial": "",
            "IdObjetivoCompra365": "",
            "NomeObjetivoCompra": "",
            "Logradouro": "",
            "ComplementoEndereco": "",
            "Idade": "",
            "IdTipoEmpreendimento365": "",
            "NomeTipoEmpreendimento": "",
            "InicioIntervaloValorImovel": "",
            "FimIntervaloValorImovel": "",
            "InicioIntervaloMetragemImovel": "",
            "FimIntervaloMetragemImovel": "",
            "QuantidadesDormitorios": "",
            "IdPrazoEntregaImovel365": "",
            "NomePrazoEntregaImovel": "",
            "IdsBairroEmpreendimento365": "",
            "BairrosEmpreendimentos": "",
            "EmpreendimentosInteresse": [
                {
                    "Id": ""
                },
                {
                    "Nome": ""
                }
            ],
            "IdentificaCliente": "false",
            "LeadRecebidoPorTransferencia": "",
            "DataTransferencia": "",
            "NomeEquipe": "",
            "IdEquipe": "",
            "Integrado": "",
            "DataHoraIntegracao": "",
            "TipoIntegracao": "",
            "Count": "0",
            "id": "LeadCorretor|1aad3f03-bb78-4002-b1e0-8de27f3fe5f1",
            "Discriminator": "LeadCorretor",
            "Id": "1aad3f03-bb78-4002-b1e0-8de27f3fe5f1",
            "Id2": "93afb081-4ffe-4fa5-bdb4-1f213bef5d3e",
            "DataAtualizacao": "2024-06-18T14:09:00.6505839Z",
            "IdUsuarioCriacao": "",
            "IdUsuarioAtualizacao": "",
            "Ativo": "true",
            "Notifications": "",
            "IsValid": "true",
            "format": "json",
            "controller": "grupo_cyrela_vivazs",
            "action": "grupo_cyrela_vivaz",
            "grupo_cyrela_vivaz": {
                "id": "LeadCorretor|1aad3f03-bb78-4002-b1e0-8de27f3fe5f1"
            }
        }
    }
}
```
## Destino: 1v1 Connect
#### Recebe dos dados e faz as seguinte verificações:

- Se ``novo_deal=sim`` existir na url, força ação de **criar** novo deal no Sellit.
- Se `novo_deal=sim`` não existir na url, então procura o deal no sellit, se encontrar atualiza todos os deals, caso não encontre cria um novo deal.
- Se ``broker_cpf`` estiver preenchido será considerado como ``cpfProprietario`` para criar ou atualizar um deal.
- Se ``broker_cpf`` não tiver dados, verifica se ``broker_whatsapp`` possue dados e verifica o cpf do corretor no DB usando como chave de busca o ``broker_whatsapp``
- Se ``token_rd`` existe na query da url pega o token informado e grava conversão no RD Station.
- Se ``observacoes`` existir na query da url, será considerado como ``observacoes``, caso contrário consdiera ``obs`` em ``lead.obs`` no JSON NL.
- Se ``origin`` existir na query da url, será adicionado ao ``DB log``

## FLUXO DE AÇÕES:
## ``Dados NL``->``1v1 Connect``->``Cria/Atualiza Sellit``->``DB log``->Conversion RD Station
#### EXEMPLO URL COMPLETA

``` 
https://1v1connect.com/api/clients/vivaz/sellit/index.php?observacoes=origem_tiktok&token_rd=3a2c488e22aeb2c23c41197b40962cef&novo_deal=sim&origin=testeorr
```

#### Querys
- token_rd
- observacoes
- novo_deal
- ogirin
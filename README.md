# API para o serviço de Confiabilidade

Essa API irá receber requisições de processamento de issues de repositório.
Quando o processamento acabar, será retornado uma URL de uma image com o gráfico resultante do processamento.
Enquanto o processamento estiver sendo efetuado, o requisitante poderá conferir o status do processamento através do uso de um token de consulta de status.

<br><br>

## GET /issues
#### Parameters
| Name | Type | Description |
|:----:|:----:|:-----------:|
| github_token | ```string```  | Token para acessar a API do Github |
| url | ```string```  | URL para o repositório que será analisado|
| must_have_labels | ```lista de string```  | Labels que indicam que determinada issue é uma issue de bug |
| must_not_have_labels | ```lista de string```  | Labels que indicam que determinada issue não deve ser considerada um bug |


#### Exemplo
```json
{
    "github_token": "<GITHUB TOKEN>",
    "url": "https://github.com/facebook/react",
    "must_have_labels": ["Type: Bug", "bug"],
    "must_not_have_labels": ["Status: Unconfirmed"]
}
```

GITHUB TOKEN: É preciso criar um token em https://github.com/settings/tokens, para aumentar o limite de requisições realizadas para a API do Github. Não é preciso de nenhuma autorização.



#### Reponse
| Name | Type | Description |
|:----:|:----:|:-----------:|
| message | ```string```  | Mensagem para o status do processamento |
| link | ```string```  | Link para acompanhar o processamento da requisição |

#### Exemplo
```json
{
    "message": "The repository has been successfully added to the processing queue! Follow the link to check the processing status.",
    "link": "https://reliability-django.herokuapp.com/processing?status_token=72e662d5-fe11-482d-bae1-bcfb9ae1ea7d"
}
```



<br><br>

## GET /processing?status_token=<STATUS_TOKEN>

#### Exemplo
```
/processing?status_token=72e662d5-fe11-482d-bae1-bcfb9ae1ea7d
```

#### Reponse

Essa rota irá retornar o status do processamento, caso o processamento não tenha terminado.
Caso o processamento já tenha terminado, será retornado o link para a image gerada.


#### Exemplo processamento em andamento
```json
{
    "message": "This repository is still being processed. ",
    "status": {
        "ERRORS_MSG": [],
        "ERRORS": false,
        "total_of_issues": 377,
        "progress": "2.12%",
        "issues_processed": 8
    }
}
```

#### Exemplo processamento finalizado
```json
{
    "message": "The repository has been successfully added to the processing queue! Follow the link to check the processing status.",
    "link": "https://reliability-images.s3-sa-east-1.amazonaws.com/4BPIXN5UAZ.png"
}
```
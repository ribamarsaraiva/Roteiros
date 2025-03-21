# API para utilização pelo sistema GPS

Essa API foi implementada para ser utilizada pelo sistema Galera_APP.

## Endpoints

1. galeraApp/colaborador/enviar
2. galeraApp/colaborador/enviarTodos
3. galeraApp/colaborador/obter/{login}
4. galeraApp/colaborador/atualizar
5. galeraApp/colaborador/atualizarTodos
6. galeraApp/colaborador/listar
7. galeraApp/colaborador/cargaTabelaControle

### URL da documentação da API no desenvolvimento
```http
https://horus-api.localhost/galeraApp/doc
```

### URL base de produção
```http
https://horus-api.kb.mpf.mp.br/GaleraApp/doc
```

**Importante:**
`
Os dados serão retornados em formato de "string", ainda que numéricos ou datas, sendo delimitados por aspas duplas; com exceção de tipo booleano (true/false) e nulo (null). Portanto, a coluna "Tipo", nos quadros descritivos de dados de resposta (response data | tem caráter informativo acerca do conteúdo, não representando o real tipo do dado.
`

## 1. Regras de negócio

#### As operações de consulta e alteração de dados são efetuadas tendo como base a tabela de controle sp.galera_app.
#### A tabela sp.galera_app guarda as informações de pessoal que serão enviadas ao sistema Galera.APP. As operações de alteração de dados nessa tabela são efetuadas via procedure de banco (pkg_galera_app.prc_carga_api_galera):
    * prc_carga_api_galera:
    Executa a atualização de registros que já foram enviados ao sistema ou Insere registros novos.

### 1.1 ação inserir
    Ocorre quando o servidor ainda não existe na tabela de controle sp.galera_app.

### 1.2 ação alterar
    Ocorre quando o servidor já existe no sistema Galera.App, porém sofreu alguma alteração que enseja o reenvio.

## 2. Colaborador: enviar

    Inclui as informações do colaborador no Galera.APP para uma matrícula especificada que está pendente de inclusão na tabela de controle.

```http
POST galeraApp/colaborador/enviar/
```
    
### Request body (Parâmetros no corpo da Url)

Atributos obrigatórios:

| Atributo |  Tipo     |  Tamanho |  Máscara | Descrição |
| -------- | --------- | ---------|--------- | --------- |
| idMatricula | integer | | | Obrigatório. <br /> Matrícula do colaborador |

### Response Data (Dados da Resposta)

| Atributo | Tipo      | Tamanho | Máscara | Descrição |
| -------- | --------- |------------- |---------- |---------- |
| name | alfanumérico | 100 | | Nome do colaborador |
| login | alfanumérico | 200 | | Login do colaborador -> campo de busca para o aplicativo Galera_app |
| registration | numérico | | Matrícula do colaborador |
| cpf | alfanumérico | 11 | 99999999999 | Número do CPF do Colaborador |
| admissiondate | date | 10 | DD/MM/AAAA | Data de admissão do Colaborador |
| resignationdate | date | 10 | DD/MM/AAAA | Data de demissão do Colaborador |
| lastsalary | numérico |  | | Valor do último salário do Colaborador |
| ddd_celphone | numérico | | | Número do código DDD do Colaborador |
| celphone | numérico |  | | Número do telefone celular do ColaboradorNúmero do DDD do telefone fixo do Colaborador |
| ddd_telephone | numérico | | | Número do DDD do telefone fixo do Colaborador |
| telephone | numérico | | | Número do telefone fixo do Colaborador |
| email | alfanumérico | 200 | | Email do colaborador |
| professional_experience | alfanumérico | 1000 | | Descrição da experiência profissional do colaborador |
| regmaritalstatus | numérico | | | Código do estado civil do colaborador |
| regposition | numérico | | | Código da função do colaborador |
| position | alfanumérico | 200 | | Descrição da função do colaborador |
| regoccupation | numérico | | | Código do cargo do colaborador |
| occupation | alfanumérico | 200 | | Descrição do cargo do colaborador |
| regdepartment | numérico | | | Código do setor do colaborador |
| department | alfanumérico | 200 | | Descrição do setor do colaborador |
| regsubsidiary | numérico | | | Código da filial do colaborador |
| subsidiary | alfanumérico | 200 | | Descrição da filial do colaborador |
| idcostcenter | numérico | | | Código do centro de custo do colaborador |
| avaliableavd | numérico | | | Indica se o colaborador será avaliado ou não (1-Sim/0-Não) |
| comments | alfanumérico | 200 | | Comentário acerca da avaliação do colaborador |
| user_admin |  numérico | | | Indica se o colaborador possui perfil de administrador no galera_app (1-Sim/0-Não) |

### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Colaborador enviado com sucesso!",
  "total": 0,
  "data": [
    {
      "name": "string",
      "login": "string",
      "registration": "string",
      "cpf": "string",
      "gender": "string",
      "birthdate": "2025-03-21",
      "admissiondate": "2025-03-21",
      "resignationdate": "2025-03-21",
      "lastsalary": 0,
      "street": "string",
      "street_number": 0,
      "address_addon": "string",
      "district": "string",
      "city": "string",
      "state": "string",
      "zip_code": "string",
      "ddd_telephone": "string",
      "telephone": "string",
      "ddd_celphone": "string",
      "celphone": "string",
      "email": "string",
      "professional_experience": "string",
      "regmaritalstatus": "string",
      "maritalstatus": "string",
      "regeducationlevel": "string",
      "educationlevel": "string",
      "regoccupation": "string",
      "occupation": "string",
      "regposition": "string",
      "position": "string",
      "regdepartment": "string",
      "department": "string",
      "regsubsidiary": "string",
      "subsidiary": "string",
      "reggroupscompany": "string",
      "groupscompany": "string",
      "regcostcenter": "string",
      "costcenter": "string",
      "avaliableavd": true,
      "comments": "string",
      "user_admin": "string"
    }
  ]
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

## 3. Colaborador: enviarTodos

    Inclui as informações de todos os colaboradores no Galera.APP que estão pendente de inclusão na tabela de controle.

```http
POST galeraApp/colaborador/enviarTodos/
```

### Response Data (Dados da Resposta)

| Atributo | Tipo      | Tamanho | Máscara | Descrição |
| -------- | --------- |------------- |---------- |---------- |
| name | alfanumérico | 100 | | Nome do colaborador |
| login | alfanumérico | 200 | | Login do colaborador -> campo de busca para o aplicativo Galera_app |
| registration | numérico | | Matrícula do colaborador |
| cpf | alfanumérico | 11 | 99999999999 | Número do CPF do Colaborador |
| admissiondate | date | 10 | DD/MM/AAAA | Data de admissão do Colaborador |
| resignationdate | date | 10 | DD/MM/AAAA | Data de demissão do Colaborador |
| lastsalary | numérico |  | | Valor do último salário do Colaborador |
| ddd_celphone | numérico | | | Número do código DDD do Colaborador |
| celphone | numérico |  | | Número do telefone celular do ColaboradorNúmero do DDD do telefone fixo do Colaborador |
| ddd_telephone | numérico | | | Número do DDD do telefone fixo do Colaborador |
| telephone | numérico | | | Número do telefone fixo do Colaborador |
| email | alfanumérico | 200 | | Email do colaborador |
| professional_experience | alfanumérico | 1000 | | Descrição da experiência profissional do colaborador |
| regmaritalstatus | numérico | | | Código do estado civil do colaborador |
| regposition | numérico | | | Código da função do colaborador |
| position | alfanumérico | 200 | | Descrição da função do colaborador |
| regoccupation | numérico | | | Código do cargo do colaborador |
| occupation | alfanumérico | 200 | | Descrição do cargo do colaborador |
| regdepartment | numérico | | | Código do setor do colaborador |
| department | alfanumérico | 200 | | Descrição do setor do colaborador |
| regsubsidiary | numérico | | | Código da filial do colaborador |
| subsidiary | alfanumérico | 200 | | Descrição da filial do colaborador |
| idcostcenter | numérico | | | Código do centro de custo do colaborador |
| avaliableavd | numérico | | | Indica se o colaborador será avaliado ou não (1-Sim/0-Não) |
| comments | alfanumérico | 200 | | Comentário acerca da avaliação do colaborador |
| user_admin |  numérico | | | Indica se o colaborador possui perfil de administrador no galera_app (1-Sim/0-Não) |

### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Colaboradores enviados com sucesso!",
  "total": 0,
  "data": [
    {
      "name": "string",
      "login": "string",
      "registration": "string",
      "cpf": "string",
      "gender": "string",
      "birthdate": "2025-03-21",
      "admissiondate": "2025-03-21",
      "resignationdate": "2025-03-21",
      "lastsalary": 0,
      "street": "string",
      "street_number": 0,
      "address_addon": "string",
      "district": "string",
      "city": "string",
      "state": "string",
      "zip_code": "string",
      "ddd_telephone": "string",
      "telephone": "string",
      "ddd_celphone": "string",
      "celphone": "string",
      "email": "string",
      "professional_experience": "string",
      "regmaritalstatus": "string",
      "maritalstatus": "string",
      "regeducationlevel": "string",
      "educationlevel": "string",
      "regoccupation": "string",
      "occupation": "string",
      "regposition": "string",
      "position": "string",
      "regdepartment": "string",
      "department": "string",
      "regsubsidiary": "string",
      "subsidiary": "string",
      "reggroupscompany": "string",
      "groupscompany": "string",
      "regcostcenter": "string",
      "costcenter": "string",
      "avaliableavd": true,
      "comments": "string",
      "user_admin": "string"
    }
  ]
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

## 4. Colaborador: obter

    Consulta dados de colaborador no Galera.APP para um Email especificado.

```http
GET galeraApp/colaborador/obter/{login}
```

### URL Params (Parâmetros de URL)

| Atributo |  Tipo     |  Tamanho |  Máscara | Descrição |
| -------- | --------- | ---------|--------- | --------- |
|   login |  String | | | Obrigatório. <br /> Email do colaborador.|


### Response Data (Dados da Resposta)

| Atributo | Tipo      | Tamanho | Máscara | Descrição |
| -------- | --------- |------------- |---------- |---------- |
| name | alfanumérico | 100 | | Nome do colaborador |
| login | alfanumérico | 200 | | Login do colaborador -> campo de busca para o aplicativo Galera_app |
| registration | numérico | | Matrícula do colaborador |
| cpf | alfanumérico | 11 | 99999999999 | Número do CPF do Colaborador |
| admissiondate | date | 10 | DD/MM/AAAA | Data de admissão do Colaborador |
| resignationdate | date | 10 | DD/MM/AAAA | Data de demissão do Colaborador |
| lastsalary | numérico |  | | Valor do último salário do Colaborador |
| ddd_celphone | numérico | | | Número do código DDD do Colaborador |
| celphone | numérico |  | | Número do telefone celular do ColaboradorNúmero do DDD do telefone fixo do Colaborador |
| ddd_telephone | numérico | | | Número do DDD do telefone fixo do Colaborador |
| telephone | numérico | | | Número do telefone fixo do Colaborador |
| email | alfanumérico | 200 | | Email do colaborador |
| professional_experience | alfanumérico | 1000 | | Descrição da experiência profissional do colaborador |
| regmaritalstatus | numérico | | | Código do estado civil do colaborador |
| regposition | numérico | | | Código da função do colaborador |
| position | alfanumérico | 200 | | Descrição da função do colaborador |
| regoccupation | numérico | | | Código do cargo do colaborador |
| occupation | alfanumérico | 200 | | Descrição do cargo do colaborador |
| regdepartment | numérico | | | Código do setor do colaborador |
| department | alfanumérico | 200 | | Descrição do setor do colaborador |
| regsubsidiary | numérico | | | Código da filial do colaborador |
| subsidiary | alfanumérico | 200 | | Descrição da filial do colaborador |
| idcostcenter | numérico | | | Código do centro de custo do colaborador |
| avaliableavd | numérico | | | Indica se o colaborador será avaliado ou não (1-Sim/0-Não) |
| comments | alfanumérico | 200 | | Comentário acerca da avaliação do colaborador |
| user_admin |  numérico | | | Indica se o colaborador possui perfil de administrador no galera_app (1-Sim/0-Não) |

### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Consulta realizada com sucesso.",
  "total": 0,
  "data": [
    {
      "name": "string",
      "login": "string",
      "registration": "string",
      "cpf": "string",
      "gender": "string",
      "birthdate": "2025-03-21",
      "admissiondate": "2025-03-21",
      "resignationdate": "2025-03-21",
      "lastsalary": 0,
      "street": "string",
      "street_number": 0,
      "address_addon": "string",
      "district": "string",
      "city": "string",
      "state": "string",
      "zip_code": "string",
      "ddd_telephone": "string",
      "telephone": "string",
      "ddd_celphone": "string",
      "celphone": "string",
      "email": "string",
      "professional_experience": "string",
      "regmaritalstatus": "string",
      "maritalstatus": "string",
      "regeducationlevel": "string",
      "educationlevel": "string",
      "regoccupation": "string",
      "occupation": "string",
      "regposition": "string",
      "position": "string",
      "regdepartment": "string",
      "department": "string",
      "regsubsidiary": "string",
      "subsidiary": "string",
      "reggroupscompany": "string",
      "groupscompany": "string",
      "regcostcenter": "string",
      "costcenter": "string",
      "avaliableavd": true,
      "comments": "string",
      "user_admin": "string"
    }
  ]
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

## 5. Colaborador: atualizar

     Inclui as informações do colaborador no Galera.APP para uma matrícula especificada pendente de atualização na tabela de controle.

```http
PUT galeraApp/colaborador/atualizar/
```
    
### Request body (Parâmetros no corpo da Url)

Atributos obrigatórios:

| Atributo |  Tipo     |  Tamanho |  Máscara | Descrição |
| -------- | --------- | ---------|--------- | --------- |
| idMatricula | integer | | | Obrigatório. <br /> Matrícula do colaborador |

### Response Data (Dados da Resposta)

| Atributo | Tipo      | Tamanho | Máscara | Descrição |
| -------- | --------- |------------- |---------- |---------- |
| name | alfanumérico | 100 | | Nome do colaborador |
| login | alfanumérico | 200 | | Login do colaborador -> campo de busca para o aplicativo Galera_app |
| registration | numérico | | Matrícula do colaborador |
| cpf | alfanumérico | 11 | 99999999999 | Número do CPF do Colaborador |
| admissiondate | date | 10 | DD/MM/AAAA | Data de admissão do Colaborador |
| resignationdate | date | 10 | DD/MM/AAAA | Data de demissão do Colaborador |
| lastsalary | numérico |  | | Valor do último salário do Colaborador |
| ddd_celphone | numérico | | | Número do código DDD do Colaborador |
| celphone | numérico |  | | Número do telefone celular do ColaboradorNúmero do DDD do telefone fixo do Colaborador |
| ddd_telephone | numérico | | | Número do DDD do telefone fixo do Colaborador |
| telephone | numérico | | | Número do telefone fixo do Colaborador |
| email | alfanumérico | 200 | | Email do colaborador |
| professional_experience | alfanumérico | 1000 | | Descrição da experiência profissional do colaborador |
| regmaritalstatus | numérico | | | Código do estado civil do colaborador |
| regposition | numérico | | | Código da função do colaborador |
| position | alfanumérico | 200 | | Descrição da função do colaborador |
| regoccupation | numérico | | | Código do cargo do colaborador |
| occupation | alfanumérico | 200 | | Descrição do cargo do colaborador |
| regdepartment | numérico | | | Código do setor do colaborador |
| department | alfanumérico | 200 | | Descrição do setor do colaborador |
| regsubsidiary | numérico | | | Código da filial do colaborador |
| subsidiary | alfanumérico | 200 | | Descrição da filial do colaborador |
| idcostcenter | numérico | | | Código do centro de custo do colaborador |
| avaliableavd | numérico | | | Indica se o colaborador será avaliado ou não (1-Sim/0-Não) |
| comments | alfanumérico | 200 | | Comentário acerca da avaliação do colaborador |
| user_admin |  numérico | | | Indica se o colaborador possui perfil de administrador no galera_app (1-Sim/0-Não) |

### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Dados atualizados com sucesso!",
  "total": 0,
  "data": [
    {
      "name": "string",
      "login": "string",
      "registration": "string",
      "cpf": "string",
      "gender": "string",
      "birthdate": "2025-03-21",
      "admissiondate": "2025-03-21",
      "resignationdate": "2025-03-21",
      "lastsalary": 0,
      "street": "string",
      "street_number": 0,
      "address_addon": "string",
      "district": "string",
      "city": "string",
      "state": "string",
      "zip_code": "string",
      "ddd_telephone": "string",
      "telephone": "string",
      "ddd_celphone": "string",
      "celphone": "string",
      "email": "string",
      "professional_experience": "string",
      "regmaritalstatus": "string",
      "maritalstatus": "string",
      "regeducationlevel": "string",
      "educationlevel": "string",
      "regoccupation": "string",
      "occupation": "string",
      "regposition": "string",
      "position": "string",
      "regdepartment": "string",
      "department": "string",
      "regsubsidiary": "string",
      "subsidiary": "string",
      "reggroupscompany": "string",
      "groupscompany": "string",
      "regcostcenter": "string",
      "costcenter": "string",
      "avaliableavd": true,
      "comments": "string",
      "user_admin": "string"
    }
  ]
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

## 6. Colaborador: atualizarTodos

    Atualiza as informações de todos os colaboradores no Galera.APP que se encontram pendentes de atualização na tabela de controle.

```http
PUT galeraApp/colaborador/atualizarTodos/
```

### Response Data (Dados da Resposta)

| Atributo | Tipo      | Tamanho | Máscara | Descrição |
| -------- | --------- |------------- |---------- |---------- |
| name | alfanumérico | 100 | | Nome do colaborador |
| login | alfanumérico | 200 | | Login do colaborador -> campo de busca para o aplicativo Galera_app |
| registration | numérico | | Matrícula do colaborador |
| cpf | alfanumérico | 11 | 99999999999 | Número do CPF do Colaborador |
| admissiondate | date | 10 | DD/MM/AAAA | Data de admissão do Colaborador |
| resignationdate | date | 10 | DD/MM/AAAA | Data de demissão do Colaborador |
| lastsalary | numérico |  | | Valor do último salário do Colaborador |
| ddd_celphone | numérico | | | Número do código DDD do Colaborador |
| celphone | numérico |  | | Número do telefone celular do ColaboradorNúmero do DDD do telefone fixo do Colaborador |
| ddd_telephone | numérico | | | Número do DDD do telefone fixo do Colaborador |
| telephone | numérico | | | Número do telefone fixo do Colaborador |
| email | alfanumérico | 200 | | Email do colaborador |
| professional_experience | alfanumérico | 1000 | | Descrição da experiência profissional do colaborador |
| regmaritalstatus | numérico | | | Código do estado civil do colaborador |
| regposition | numérico | | | Código da função do colaborador |
| position | alfanumérico | 200 | | Descrição da função do colaborador |
| regoccupation | numérico | | | Código do cargo do colaborador |
| occupation | alfanumérico | 200 | | Descrição do cargo do colaborador |
| regdepartment | numérico | | | Código do setor do colaborador |
| department | alfanumérico | 200 | | Descrição do setor do colaborador |
| regsubsidiary | numérico | | | Código da filial do colaborador |
| subsidiary | alfanumérico | 200 | | Descrição da filial do colaborador |
| idcostcenter | numérico | | | Código do centro de custo do colaborador |
| avaliableavd | numérico | | | Indica se o colaborador será avaliado ou não (1-Sim/0-Não) |
| comments | alfanumérico | 200 | | Comentário acerca da avaliação do colaborador |
| user_admin |  numérico | | | Indica se o colaborador possui perfil de administrador no galera_app (1-Sim/0-Não) |

### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Dados atualizados com sucesso.",
  "total": 0,
  "data": [
    {
      "name": "string",
      "login": "string",
      "registration": "string",
      "cpf": "string",
      "gender": "string",
      "birthdate": "2025-03-21",
      "admissiondate": "2025-03-21",
      "resignationdate": "2025-03-21",
      "lastsalary": 0,
      "street": "string",
      "street_number": 0,
      "address_addon": "string",
      "district": "string",
      "city": "string",
      "state": "string",
      "zip_code": "string",
      "ddd_telephone": "string",
      "telephone": "string",
      "ddd_celphone": "string",
      "celphone": "string",
      "email": "string",
      "professional_experience": "string",
      "regmaritalstatus": "string",
      "maritalstatus": "string",
      "regeducationlevel": "string",
      "educationlevel": "string",
      "regoccupation": "string",
      "occupation": "string",
      "regposition": "string",
      "position": "string",
      "regdepartment": "string",
      "department": "string",
      "regsubsidiary": "string",
      "subsidiary": "string",
      "reggroupscompany": "string",
      "groupscompany": "string",
      "regcostcenter": "string",
      "costcenter": "string",
      "avaliableavd": true,
      "comments": "string",
      "user_admin": "string"
    }
  ]
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

## 7. Colaborador: listar

    Lista dados de todos os colaboradores existentes no Galera.APP.

```http
GET galeraApp/colaborador/listar
```

### Response Data (Dados da Resposta)

| Atributo | Tipo      | Tamanho | Máscara | Descrição |
| -------- | --------- |------------- |---------- |---------- |
| name | alfanumérico | 100 | | Nome do colaborador |
| login | alfanumérico | 200 | | Login do colaborador -> campo de busca para o aplicativo Galera_app |
| registration | numérico | | Matrícula do colaborador |
| cpf | alfanumérico | 11 | 99999999999 | Número do CPF do Colaborador |
| admissiondate | date | 10 | DD/MM/AAAA | Data de admissão do Colaborador |
| resignationdate | date | 10 | DD/MM/AAAA | Data de demissão do Colaborador |
| lastsalary | numérico |  | | Valor do último salário do Colaborador |
| ddd_celphone | numérico | | | Número do código DDD do Colaborador |
| celphone | numérico |  | | Número do telefone celular do ColaboradorNúmero do DDD do telefone fixo do Colaborador |
| ddd_telephone | numérico | | | Número do DDD do telefone fixo do Colaborador |
| telephone | numérico | | | Número do telefone fixo do Colaborador |
| email | alfanumérico | 200 | | Email do colaborador |
| professional_experience | alfanumérico | 1000 | | Descrição da experiência profissional do colaborador |
| regmaritalstatus | numérico | | | Código do estado civil do colaborador |
| regposition | numérico | | | Código da função do colaborador |
| position | alfanumérico | 200 | | Descrição da função do colaborador |
| regoccupation | numérico | | | Código do cargo do colaborador |
| occupation | alfanumérico | 200 | | Descrição do cargo do colaborador |
| regdepartment | numérico | | | Código do setor do colaborador |
| department | alfanumérico | 200 | | Descrição do setor do colaborador |
| regsubsidiary | numérico | | | Código da filial do colaborador |
| subsidiary | alfanumérico | 200 | | Descrição da filial do colaborador |
| idcostcenter | numérico | | | Código do centro de custo do colaborador |
| avaliableavd | numérico | | | Indica se o colaborador será avaliado ou não (1-Sim/0-Não) |
| comments | alfanumérico | 200 | | Comentário acerca da avaliação do colaborador |
| user_admin |  numérico | | | Indica se o colaborador possui perfil de administrador no galera_app (1-Sim/0-Não) |

### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Consulta realizada com sucesso.",
  "total": 0,
  "data": [
    {
      "name": "string",
      "login": "string",
      "registration": "string",
      "cpf": "string",
      "gender": "string",
      "birthdate": "2025-03-21",
      "admissiondate": "2025-03-21",
      "resignationdate": "2025-03-21",
      "lastsalary": 0,
      "street": "string",
      "street_number": 0,
      "address_addon": "string",
      "district": "string",
      "city": "string",
      "state": "string",
      "zip_code": "string",
      "ddd_telephone": "string",
      "telephone": "string",
      "ddd_celphone": "string",
      "celphone": "string",
      "email": "string",
      "professional_experience": "string",
      "regmaritalstatus": "string",
      "maritalstatus": "string",
      "regeducationlevel": "string",
      "educationlevel": "string",
      "regoccupation": "string",
      "occupation": "string",
      "regposition": "string",
      "position": "string",
      "regdepartment": "string",
      "department": "string",
      "regsubsidiary": "string",
      "subsidiary": "string",
      "reggroupscompany": "string",
      "groupscompany": "string",
      "regcostcenter": "string",
      "costcenter": "string",
      "avaliableavd": true,
      "comments": "string",
      "user_admin": "string"
    }
  ]
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

## 8. Colaborador: cargaTabelaControle

    Atualiza a tabela de controle.

```http
PUT galeraApp/colaborador/cargaTabelaControle/
```
    
### Request body (Parâmetros no corpo da Url)

Atributos opcionais:

| Atributo |  Tipo     |  Tamanho |  Máscara | Descrição |
| -------- | --------- | ---------|--------- | --------- |
| idMatricula | integer | | | Matrícula do colaborador |


### Success response

```json
//status: 200 OK
{
  "success": true,
  "message": "Carga realizada com sucesso."
}
```

### Failure response

```json
//status: 400 Bad Request
{
  "success": false,
  "message": "O recurso solicitado não existe."
}

//status: 500 Internal Server Error
{
  "success": false,
  "message": "Falha ao consultar informações no banco de dados."
}
```

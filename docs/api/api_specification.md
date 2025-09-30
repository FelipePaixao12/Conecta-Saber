# Especificação da API – Plataforma Conecta Saber 🧑‍🎓

Este documento detalha a especificação técnica da API **Conecta Saber**, incluindo seus endpoints centrais para **agendamento, frequência e perfis**. Ele serve como um guia para desenvolvedores que constroem as interfaces Mobile (Aluno/Voluntário) e Web (Professor/Gestor).

## 1\. Endpoints Previstos

### 1.1. `POST /api/auth/login`

  * **Descrição**: Realiza a autenticação de qualquer perfil de usuário (Aluno, Voluntário, Professor, Administrador) e emite um token JWT.
  * **Autenticação**: Não requer autenticação.

### 1.2. `GET /api/aulas/disponiveis`

  * **Descrição**: Retorna a lista de ofertas de aulas de reforço cadastradas por voluntários, com filtros por disciplina, escola e data. (Endpoint principal para o **Aluno**).
  * **Autenticação**: Requer um token de autenticação (JWT).

### 1.3. `POST /api/frequencia`

  * **Descrição**: Permite que o **Voluntário** registre a presença e o *feedback* de um Aluno em uma aula concluída (RF04).
  * **Autenticação**: Requer um token de autenticação (JWT) e autorização de perfil de Voluntário.

### 1.4. `GET /api/health`

  * **Descrição**: Endpoint de verificação de saúde da API. Usado para monitorar se a aplicação está online. Não requer autenticação.

-----

## 2\. Autenticação e Autorização

A API utiliza o padrão de autenticação por **token (JWT)**. Para acessar a maioria dos endpoints, o cliente deve incluir um token de autorização no cabeçalho da requisição.

  * **Método**: Bearer Token
  * **Formato**: O token de autenticação (JWT) deve ser enviado no cabeçalho `Authorization`.

**Exemplo de Cabeçalho de Requisição:**

```
Authorization: Bearer <seu_token_jwt>
Content-Type: application/json
```

**Observação**: Apenas os endpoints de login e `/api/health` não exigem autenticação.

-----

## 3\. Detalhamento de Endpoints

### 3.1. `POST /api/auth/login`

#### Parâmetros de Requisição

Este endpoint espera um corpo de requisição no formato JSON.

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `email` | `string` | Sim | E-mail do usuário cadastrado. |
| `senha` | `string` | Sim | Senha do usuário. |

**Exemplo de Requisição (Body):**

```json
{
  "email": "aluno.joao@escola.edu.br",
  "senha": "senhaSegura123"
}
```

#### Formatos de Resposta

##### Resposta de Sucesso (Status Code: `200 OK`)

A resposta retorna o token JWT e os dados básicos do usuário.

**Exemplo de Resposta (Body):**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "usuario": {
    "id": 102,
    "nome": "João da Silva",
    "tipo_perfil": "Aluno"
  },
  "status": "success"
}
```

##### Resposta de Erro (Status Code: `401 Unauthorized`)

Ocorre quando as credenciais (e-mail ou senha) são inválidas.

**Exemplo de Resposta (Body):**

```json
{
  "error": "Credenciais inválidas. Verifique seu e-mail e senha.",
  "status": "error"
}
```

-----

### 3.2. `GET /api/aulas/disponiveis`

#### Parâmetros de Requisição (Query Params)

Este endpoint aceita filtros opcionais via URL.

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `disciplina` | `string` | Não | Filtra aulas por nome da disciplina (Ex: "Matemática"). |
| `escola_id` | `integer` | Não | Filtra aulas oferecidas em uma escola parceira específica. |
| `data` | `date` | Não | Filtra aulas a partir de uma data específica (`YYYY-MM-DD`). |

**Exemplo de Requisição (URL):**

`GET /api/aulas/disponiveis?disciplina=Português&escola_id=5`

#### Formatos de Resposta

##### Resposta de Sucesso (Status Code: `200 OK`)

Retorna uma lista de aulas disponíveis para agendamento.

**Exemplo de Resposta (Body):**

```json
{
  "aulas": [
    {
      "id_oferta": 201,
      "disciplina": "Matemática",
      "data_hora": "2025-10-05T15:00:00Z",
      "voluntario_nome": "Pedro Alves",
      "escola_nome": "E.E.F. Maria Helena",
      "modalidade": "presencial"
    }
  ],
  "total": 15
}
```

-----

### 3.3. `POST /api/frequencia`

#### Parâmetros de Requisição

Este endpoint espera um corpo de requisição no formato JSON.

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `agendamento_id` | `integer` | Sim | ID do Agendamento que está sendo finalizado (PK da tabela `Agendamento`). |
| `presente` | `boolean` | Sim | Indica se o aluno compareceu à aula. |
| `feedback` | `string` | Não | Observações do Voluntário sobre o desempenho na aula (máximo 500 caracteres). |

**Exemplo de Requisição (Body):**

```json
{
  "agendamento_id": 35,
  "presente": true,
  "feedback": "Aluno demonstrou ótimo entendimento em frações e fez todas as atividades propostas."
}
```

#### Formatos de Resposta

##### Resposta de Sucesso (Status Code: `201 Created`)

Indica que a frequência e o feedback foram registrados com sucesso na tabela `Frequencia`.

**Exemplo de Resposta (Body):**

```json
{
  "mensagem": "Frequência registrada e agendamento concluído com sucesso.",
  "status": "success"
}
```

-----

### 3.4. `GET /api/health`

#### Parâmetros de Requisição

Este endpoint não requer nenhum parâmetro.

#### Formatos de Resposta

##### Resposta de Sucesso (Status Code: `200 OK`)

Indica que a API está em pleno funcionamento.

**Exemplo de Resposta (Body):**

```json
{
  "status": "OK",
  "servico": "Conecta Saber API",
  "timestamp": "2025-09-29T21:39:15.000Z",
  "version": "1.0.0-mvp"
}
```

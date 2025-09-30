# Especificação da API – Plataforma Conecta Saber 🧑‍🎓

Este documento detalha a especificação técnica da API **Conecta Saber**, incluindo seus endpoints para gerenciamento de agendamentos, perfis e frequência. Ele serve como um guia para desenvolvedores *frontend* (Web e Mobile) que precisam integrar sistemas com o *backend*.

## 1\. Endpoints Previstos

### 1.1. `POST /api/auth/login`

  * **Descrição**: Realiza a autenticação de qualquer perfil de usuário (Aluno, Voluntário, Professor, Administrador) e emite um token JWT.
  * **Autenticação**: Não requer autenticação (é o ponto de entrada).

### 1.2. `GET /api/aulas/disponiveis`

  * **Descrição**: Retorna a lista de ofertas de aulas de reforço cadastradas por voluntários, permitindo filtros por disciplina e escola. Este é o endpoint principal para o **Aluno**.
  * **Autenticação**: Requer um token de autenticação (JWT).

### 1.3. `POST /api/agendamentos`

  * **Descrição**: Permite que o Aluno solicite o agendamento de uma aula disponível (RF02) ou que o Voluntário aceite uma solicitação de *matchmaking*.
  * **Autenticação**: Requer um token de autenticação (JWT).

### 1.4. `POST /api/frequencia`

  * **Descrição**: Permite que o Voluntário registre a frequência de um Aluno em uma aula concluída (RF04).
  * **Autenticação**: Requer um token de autenticação (JWT) e autorização específica de **Voluntário**.

-----

## 2\. Autenticação e Autorização

A API utiliza o padrão de autenticação por **token (JWT)** para garantir que apenas usuários válidos possam acessar os dados.

  * **Método**: Bearer Token
  * **Formato**: O token de autenticação (JWT) deve ser obtido após o login e enviado no cabeçalho `Authorization`.

**Exemplo de Cabeçalho de Requisição (Para endpoints seguros):**

```
Authorization: Bearer <seu_token_jwt>
Content-Type: application/json
```

**Observação**: O endpoint de login (`/api/auth/login`) e o de verificação de saúde (`/api/health`) não exigem autenticação.

-----

## 3\. Detalhamento de Endpoints

### 3.1. `POST /api/auth/login`

#### Parâmetros de Requisição

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `email` | `string` | Sim | E-mail do usuário cadastrado. |
| `senha` | `string` | Sim | Senha do usuário. |

**Exemplo de Requisição (Body):**

```json
{
  "email": "professor.maria@escola.edu.br",
  "senha": "senhaSegura123"
}
```

#### Formatos de Resposta (Sucesso)

##### Resposta de Sucesso (Status Code: `200 OK`)

A resposta retorna o token JWT e os dados básicos do usuário.

**Exemplo de Resposta (Body):**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "usuario": {
    "id": 101,
    "nome": "Maria de Fátima",
    "tipo_perfil": "Professor"
  }
}
```

-----

### 3.2. `GET /api/aulas/disponiveis`

#### Parâmetros de Requisição (Query Params)

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `disciplina` | `string` | Não | Filtra aulas por nome da disciplina (Ex: "Matemática"). |
| `escola_id` | `integer` | Não | Filtra aulas oferecidas em uma escola específica. |
| `data` | `date` | Não | Filtra aulas a partir de uma data específica (`YYYY-MM-DD`). |

**Exemplo de Requisição (URL):**

`GET /api/aulas/disponiveis?disciplina=Português&escola_id=5`

#### Formatos de Resposta (Sucesso)

##### Resposta de Sucesso (Status Code: `200 OK`)

Retorna uma lista paginada de aulas disponíveis.

**Exemplo de Resposta (Body):**

```json
{
  "aulas": [
    {
      "id": 201,
      "disciplina": "Matemática",
      "data_hora": "2025-10-05T15:00:00Z",
      "local": "Escola X - Sala 3",
      "voluntario_nome": "João Silva",
      "vagas_restantes": 5
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
| `agendamento_id` | `integer` | Sim | ID do agendamento que está sendo encerrado. |
| `aluno_id` | `integer` | Sim | ID do aluno cuja frequência será registrada. |
| `presente` | `boolean` | Sim | Indica se o aluno compareceu à aula. |
| `feedback` | `string` | Não | Observações do Voluntário sobre o desempenho ou a aula (máximo 500 caracteres). |

**Exemplo de Requisição (Body):**

```json
{
  "agendamento_id": 35,
  "aluno_id": 102,
  "presente": true,
  "feedback": "Aluno demonstrou ótimo entendimento em frações."
}
```

#### Formatos de Resposta (Sucesso)

##### Resposta de Sucesso (Status Code: `201 Created`)

Indica que a frequência e o feedback foram registrados com sucesso.

**Exemplo de Resposta (Body):**

```json
{
  "mensagem": "Frequência registrada com sucesso.",
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
  "timestamp": "2025-09-29T12:00:00.000Z",
  "version": "1.0.0-mvp"
}
```

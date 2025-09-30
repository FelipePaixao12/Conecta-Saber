# Modelo de Dados – Plataforma Conecta Saber 🧑‍🎓

## 1\. Introdução

Este documento detalha o **modelo de dados** da plataforma **Conecta Saber**. Ele descreve as tabelas, os campos, os tipos de dados e os relacionamentos do banco de dados. O objetivo é criar uma base sólida para as funcionalidades de **agendamento de aulas, *matchmaking* de voluntários, gestão de frequência** e **rastreamento de desempenho** educacional.

-----

## 2\. Visão Geral das Entidades

O banco de dados será composto pelas seguintes tabelas principais, focadas na gestão de pessoas e agendamentos:

  * **`Usuario`**: Tabela central de perfis (Aluno, Voluntário, Professor, Admin).
  * **`Escola`**: Informações das instituições de ensino parceiras.
  * **`VoluntarioDetalhe`**: Dados específicos de certificação e especialidade dos voluntários.
  * **`OfertaAula`**: O que o voluntário se propõe a dar (Ex: Matemática, Terças, 15h).
  * **`Agendamento`**: A instância da aula que um aluno solicitou e foi confirmada.
  * **`Frequencia`**: O registro de presença e feedback de uma aula agendada.
  * **`Disciplina`**: Lista de disciplinas que podem ser ensinadas (Ex: Português, Física).

-----

## 3\. Detalhamento das Entidades e Relacionamentos

Aqui, cada tabela é descrita com seus campos e relacionamentos, refletindo a lógica de reforço escolar.

### `Usuario`

  * **Para que serve:** Armazena dados de autenticação e tipo de perfil (`aluno`, `voluntario`, `professor`, `admin`).
  * **Relacionamentos:**
      * Um usuário possui **um** perfil detalhado (Ex: `VoluntarioDetalhe` ou `AlunoDetalhe`) (1:1).

### `Escola`

  * **Para que serve:** Gerencia as instituições parceiras e suas localizações.
  * **Relacionamentos:**
      * Uma escola **está ligada a** vários **professores/alunos** (1:N).

### `VoluntarioDetalhe`

  * **Para que serve:** Guarda informações específicas do voluntário, como status de validação e áreas de conhecimento.
  * **Relacionamentos:**
      * Um voluntário **pode criar** várias **ofertas de aulas** (1:N).

### `OfertaAula`

  * **Para que serve:** É o "serviço" de reforço que um voluntário cadastra (Ex: "Matemática básica toda segunda").
  * **Relacionamentos:**
      * Uma oferta de aula **pode gerar** vários **agendamentos** (1:N).

### `Agendamento`

  * **Para que serve:** O registro da aula marcada (voluntário X aluno X horário específico). É a entidade central da logística.
  * **Relacionamentos:**
      * Um agendamento **possui** um único registro de **frequência** (1:1).

### `Frequencia`

  * **Para que serve:** Armazena o registro de presença do aluno e o *feedback* do voluntário (RF04).
  * **Relacionamentos:**
      * Uma frequência **é registrada para** um único **agendamento** (1:1).

### `Disciplina`

  * **Para que serve:** Lista as disciplinas disponíveis para reforço.
  * **Relacionamentos:**
      * Uma disciplina **está ligada a** várias **ofertas de aulas** (1:N).

-----

## 4\. Dicionário de Dados

Esta seção detalha cada campo das tabelas, especificando o tipo de dado e as restrições.

#### **Tabela: `Usuario`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id_usuario` | `INT` | PK, AUTO\_INCREMENT | Identificador único. |
| `nome` | `VARCHAR(100)` | NOT NULL | Nome completo. |
| `email` | `VARCHAR(150)` | NOT NULL, UNIQUE | E-mail. Usado como login. |
| `senha_hash` | `VARCHAR(255)` | NOT NULL | Senha criptografada. |
| `tipo_perfil` | `ENUM` | NOT NULL | Tipo: 'aluno', 'voluntario', 'professor', 'admin'. |
| `data_cadastro` | `DATETIME` | NOT NULL | Data e hora de registro. |

#### **Tabela: `Escola`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id_escola` | `INT` | PK, AUTO\_INCREMENT | Identificador único da escola. |
| `nome` | `VARCHAR(100)` | NOT NULL | Nome da instituição. |
| `endereco` | `VARCHAR(255)` | NOT NULL | Endereço completo. |
| `diretor_id` | `INT` | FK | Chave estrangeira para o usuário que é diretor/gestor da escola. |

#### **Tabela: `VoluntarioDetalhe`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id_usuario` | `INT` | PK, FK, NOT NULL | Chave primária e estrangeira para `Usuario`. |
| `cpf` | `VARCHAR(11)` | UNIQUE | CPF para validação de segurança. |
| `formacao` | `VARCHAR(150)` | | Área de formação ou estudo. |
| `status_validacao` | `ENUM` | NOT NULL | Status: 'pendente', 'aprovado', 'rejeitado'. |

#### **Tabela: `Disciplina`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id_disciplina` | `INT` | PK, AUTO\_INCREMENT | Identificador único. |
| `nome` | `VARCHAR(50)` | NOT NULL, UNIQUE | Nome da disciplina (Ex: Matemática, Português). |

#### **Tabela: `OfertaAula`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id_oferta` | `INT` | PK, AUTO\_INCREMENT | Identificador único da oferta. |
| `voluntario_id` | `INT` | FK, NOT NULL | Chave estrangeira para o `Usuario` Voluntário. |
| `disciplina_id` | `INT` | FK, NOT NULL | Chave estrangeira para a `Disciplina` oferecida. |
| `dia_semana` | `ENUM` | NOT NULL | Dia da semana em que a aula é ofertada (Ex: Seg, Ter). |
| `horario_inicio` | `TIME` | NOT NULL | Hora de início da oferta. |
| `modalidade` | `ENUM` | NOT NULL | 'presencial' ou 'online'. |

#### **Tabela: `Agendamento`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id_agendamento` | `INT` | PK, AUTO\_INCREMENT | Identificador único do agendamento. |
| `oferta_id` | `INT` | FK, NOT NULL | Chave estrangeira para a `OfertaAula` que originou o agendamento. |
| `aluno_id` | `INT` | FK, NOT NULL | Chave estrangeira para o `Usuario` Aluno. |
| `data_agendada` | `DATETIME` | NOT NULL | Data e hora exata da aula. |
| `status` | `ENUM` | NOT NULL | 'pendente', 'confirmado', 'cancelado', 'concluido'. |
| `local_aula` | `VARCHAR(255)` | | O local onde a aula será realizada (sala/link). |

#### **Tabela: `Frequencia`**

| Campo | Tipo de Dado | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `agendamento_id` | `INT` | PK, FK, NOT NULL | Chave primária e estrangeira para `Agendamento`. |
| `status_presenca` | `ENUM` | NOT NULL | 'presente', 'falta\_justificada', 'falta\_injustificada'. |
| `feedback_voluntario` | `TEXT` | | Observações do voluntário sobre o desempenho do aluno (RF04). |
| `data_registro` | `DATETIME` | NOT NULL | Data e hora do registro. |

-----

## 5\. Diagrama Entidade-Relacionamento (DER)

Este diagrama visualiza a estrutura do banco de dados, mostrando as tabelas e seus relacionamentos, focado na gestão educacional.

```mermaid
erDiagram
    USUARIO ||--o{ ESCOLA : "gerencia"
    ESCOLA ||--o{ USUARIO : "pertence_a"
    USUARIO ||--|{ VOLUNTARIO_DETALHE : "detalha_perfil"
    
    USUARIO {
        int id_usuario PK
        string nome
        string email
        string senha_hash
        enum tipo_perfil
    }
    
    ESCOLA {
        int id_escola PK
        string nome
        string endereco
        int diretor_id FK
    }
    
    VOLUNTARIO_DETALHE {
        int id_usuario PK, FK
        string cpf UNIQUE
        string formacao
        enum status_validacao
    }
    
    VOLUNTARIO_DETALHE ||--o{ OFERTA_AULA : "cria_oferta"
    DISCIPLINA ||--o{ OFERTA_AULA : "é_sobre"
    
    OFERTA_AULA {
        int id_oferta PK
        int voluntario_id FK
        int disciplina_id FK
        enum dia_semana
        time horario_inicio
        enum modalidade
    }
    
    USUARIO ||--o{ AGENDAMENTO : "solicita_ou_é_aluno"
    OFERTA_AULA ||--o{ AGENDAMENTO : "origina"
    
    AGENDAMENTO {
        int id_agendamento PK
        int oferta_id FK
        int aluno_id FK
        datetime data_agendada
        enum status
        string local_aula
    }
    
    AGENDAMENTO ||--|| FREQUENCIA : "possui"
    
    FREQUENCIA {
        int agendamento_id PK, FK
        enum status_presenca
        text feedback_voluntario
        datetime data_registro
    }
    
    DISCIPLINA {
        int id_disciplina PK
        string nome UNIQUE
    }
```

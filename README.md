# Projeto-ConectaSaber-N705

Repositório do Projeto Aplicado N705 - Documentação técnica e protótipos.

**Tema:** Plataforma Conecta Saber: Apoio Escolar Comunitário 🧑‍🎓

-----

## 1\. Introdução

O projeto *Conecta Saber* contribui diretamente para a **ODS 4 – Educação de Qualidade**, ao focar na garantia de acesso inclusivo e equitativo à educação complementar para estudantes de escolas públicas em áreas de vulnerabilidade social (como Bom Jardim, Granja Portugal e Conjunto Palmeiras).

A iniciativa busca **diminuir a defasagem de aprendizado** e **combater a evasão escolar** por meio da tecnologia. Ao criar uma ponte digital entre voluntários qualificados (universitários, profissionais) e alunos que precisam de reforço, o sistema fortalece a comunidade e democratiza o acesso a aulas personalizadas e atividades extracurriculares.

O sistema propõe uma solução **multiplataforma** para gerenciar a complexa logística de agendamento, *matchmaking* de habilidades, validação de segurança do voluntário e acompanhamento do desempenho do aluno. Ele transforma a boa vontade da comunidade em um recurso educacional organizado e mensurável, promovendo um impacto social positivo e direto na qualidade da educação básica.

-----

## 1.1 Problema abordado e justificativa

Esse projeto visa resolver a problemática da **deficiência na oferta de reforço escolar** em comunidades carentes. Algumas questões que a sociedade enfrenta relacionadas a essa demanda:

**Inconsistência no Reforço:** Muitas vezes, o reforço escolar depende de iniciativas isoladas ou pontuais, sem continuidade, organização e rastreabilidade do impacto.

**Barreira de Acesso e *Matchmaking*:** É difícil para professores identificarem e encaminharem alunos para reforço e, para voluntários, encontrarem onde suas habilidades são mais necessárias. A plataforma resolve esse *gap* logístico.

**Falta de Dados e Acompanhamento:** Os gestores escolares não têm ferramentas fáceis para rastrear quantas horas de reforço um aluno específico recebeu ou qual o impacto desse reforço em suas notas e frequência. O sistema oferece relatórios de progresso.

## 2\. Objetivo do Sistema

Desenvolver uma plataforma **multiplataforma** (Web para gestão, Mobile para Alunos e Voluntários) que promova:

  - O **matchmaking** (cruzamento de oferta e demanda) eficiente entre voluntários e alunos.
  - O **agendamento** e o **gerenciamento** de aulas de reforço e oficinas educacionais.
  - O **acompanhamento** do progresso e da frequência dos alunos por parte de professores e gestores.

## 3\. Escopo do Projeto

O sistema **Conecta Saber** tem como objetivo apoiar a educação complementar, integrando a comunidade e as escolas públicas.

### 3.1 Escopo Incluído (MVP)

  - **Cadastro** e validação de quatro perfis: Aluno, Voluntário, Professor/Gestor e Administrador.
  - **Funcionalidade de Agendamento** de aulas de reforço presenciais (na escola) e online básicas.
  - **Painel de Controle (Web)** para professores/gestores rastrearem a frequência e o desempenho dos alunos no programa de reforço.
  - **Sistema de Notificações** (Push/Email) para lembretes de aulas e confirmações de agendamento.
  - **Mapeamento** de escolas parceiras e suas demandas por disciplina.

### 3.2 Escopo Excluído (Para Futuras Versões)

  - Pagamentos financeiros ou doações online.
  - Chat em tempo real ou módulo de videoconferência próprio para as aulas online.
  - Módulo de *e-learning* completo com videoaulas gravadas.
  - Integração automática com sistemas de gestão escolar (SGE) já utilizados pela rede pública.

-----

## 4\. Arquitetura do Sistema

O sistema é composto por frontend (mobile para Aluno/Voluntário e web para Gestão), backend (API), e banco de dados.

  - **Frontend Mobile**: Aplicativo para Alunos e Voluntários (priorizando facilidade de uso e notificações).
  - **Frontend Web**: Painel de Controle para Professores e Gestores (priorizando relatórios e gestão de dados).
  - **Backend (Microsserviços)**: API central que gerencia Agendamentos, Perfis e *Matchmaking*.
  - **APIs Externas**: Serviços de Notificação (Push Notifications) e, futuramente, APIs de Georreferenciamento.
  - **Banco de Dados**: Armazena informações de usuários, agendamentos, frequências e escolas.

\<img width="773" height="336" alt="Diagrama de Arquitetura do Sistema Conecta Saber" src="[https://github.com/user-attachments/assets/22341c07-528e-4dbc-95c4-623f3e18f5b1](https://github.com/user-attachments/assets/22341c07-528e-4dbc-95c4-623f3e18f5b1)" /\>

-----

## 5\. Tecnologias e Ferramentas

  - **Frontend Mobile**: **Flutter** ou **React Native** (Para desenvolvimento multiplataforma eficiente).
  - **Frontend Web**: **React.js** ou **Vue.js** (Para o Painel de Gestão e Relatórios).
  - **Backend**: **Node.js + Express** ou **Java + Spring Boot** (Para alta performance da API de Agendamentos).
  - **Banco de Dados**: **PostgreSQL** (Relacional, ideal para dados estruturados e transacionais como agendamentos e frequência).
  - **APIs**: Push Notification Service (Firebase ou similar), API de Georreferenciamento (Google Maps ou similar).
  - **Prototipação**: Figma.
  - **Documentação**: Markdown no GitHub.

-----

### 6\. Proposta de Cronograma para a Etapa 2 (N708)

Este cronograma foi planejado para a Etapa 2 do projeto, com duração total de **4 semanas**. As atividades estão otimizadas com foco no desenvolvimento e integração do *matchmaking* e agendamento.

| **Semana** | **Atividade** | **Descrição Detalhada** |
| :--- | :--- | :--- |
| **Semana 1** | **Configuração do Backend e Modelagem do Banco de Dados** | Foco na estrutura fundamental do sistema. |
| | | - **Dia 1-3:** Configuração do ambiente de desenvolvimento (Backend escolhido). Instalação de dependências e ferramentas. |
| | | - **Dia 4-7:** Desenvolvimento do modelo de dados. Criação das tabelas de **`Usuario`**, **`Escola`**, **`Oferta_Aula`** e **`Agendamento`**. |
| **Semana 2** | **Desenvolvimento Frontend e Backend: Perfis e Cadastro** | Início da construção das interfaces e funcionalidades principais. |
| | | - **Dia 1-4:** Desenvolvimento das telas de **Cadastro/Login** e do painel principal para Aluno e Voluntário (App Mobile). |
| | | - **Dia 5-7:** Criação dos endpoints no backend para **cadastro de perfis** e **validação de Voluntários**. |
| **Semana 3** | **Lógica de Negócio: Matchmaking e Agendamento** | A etapa central de integração e implementação da lógica de negócio. |
| | | - **Dia 1-4:** Desenvolvimento do **Serviço de Agendamento** e da lógica de **Matchmaking** (cruzamento Aluno/Voluntário). |
| | | - **Dia 5-7:** Implementação das telas de **Busca de Aulas** (Aluno) e **Aceite de Oferta** (Voluntário). |
| **Semana 4** | **Gestão, Testes Finais e Documentação** | Etapa de garantia de qualidade e preparação para a entrega. |
| | | - **Dia 1-3:** Desenvolvimento do **Painel Web do Professor/Gestor** (Relatórios de Frequência e Progresso). |
| | | - **Dia 4-5:** **Testes de Integração e Funcionalidade**. Verificação do fluxo completo do Agendamento e Notificações. |
| | | - **Dia 6-7:** Finalização da **Documentação** (código e API) e preparação para a apresentação do trabalho. |

-----

## 7\. Integrantes da Equipe e seus papeis

| Nome | Matrícula | Função |
| :--- | :--- | :--- |
| Rafael Levi Dias Vasconcelos Ponte | 2318845 | Desenvolvimento Backend |
| Elton Vasconcelos Sales de Castro Braga | 2222925 | Desenvolvimento das APIs |
| Vitor Regison Lima Machado | 2323779 | Modelagem de Dados e Arquitetura |
| Alexandre de Oliveira da Costa | 202400004 | Desenvolvimento Frontend e UX/UI |
| Felipe Paixão Lima | 2323781 | Análise de Requisitos e Documentação |

# Documento de Visão — Sistema de Gestão para Clínica de Psicologia

**Versão:** 1.1 · **Data:** 2026-06-13 · **Autora:** Amanda Barbosa Ferreira Agapito
**Disciplina:** Arquitetura e Desenho de Software — PUC Goiás · 2026.1
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 7

## Histórico de revisões

| Versão | Data       | Descrição                                                                 | Autor   |
|--------|------------|---------------------------------------------------------------------------|---------|
| 1.0    | 2026-05-07 | Versão inicial — consolidação dos requisitos da Entrega 1                  | Amanda  |
| 1.1    | 2026-06-13 | Correções da revisão da Entrega 2: stakeholder CFP, separação entre restrições / regras de negócio / evolução futura, redução de escopo (financeiro adiado), ambiente do usuário e perspectiva do produto. | Equipe |

---

## 1. Introdução

### 1.1 Propósito

Este Documento de Visão coleta, analisa e define as necessidades e características de
alto nível do **Sistema de Gestão para Clínica de Psicologia** (o *produto*).
Concentra-se nas capacidades requeridas pelos *stakeholders* e usuários-alvo e nos
motivos pelos quais essas necessidades existem. Os detalhes de **como** o sistema
atende a essas necessidades são tratados na Especificação de Requisitos (ERS — IEEE 830)
e na Especificação Suplementar (ISO/IEC 25010).

### 1.2 Escopo

O documento aplica-se ao Sistema de Gestão para Clínica de Psicologia, uma **aplicação
web responsiva com prioridade mobile (mobile-first)**, destinada a:

- **Pequenas clínicas multidisciplinares** (até ~15 profissionais, com staff administrativo);
- **Psicólogos autônomos** operando individualmente ou em espaços compartilhados.

> **Escopo desta entrega (Entrega 2).** Por orientação de revisão, o escopo foi **reduzido**
> para garantir um modelo coeso dentro do prazo. O foco é o **núcleo de agendamento,
> atendimento/prontuário e comunicação**. O **módulo financeiro** (cobrança, pagamento,
> emissão de recibo, inadimplência e relatórios financeiros) foi movido para
> **Evolução futura** (ver §7).

### 1.3 Definições, acrônimos e abreviações

- **Anamnese:** primeira entrevista clínica para coleta de histórico do paciente.
- **Sessão:** encontro terapêutico entre paciente e profissional.
- **Prontuário:** registro clínico cumulativo do paciente.
- **LGPD:** Lei Geral de Proteção de Dados Pessoais (Lei 13.709/2018).
- **CFP:** Conselho Federal de Psicologia (regulador do exercício e do prontuário psicológico).
- **RBAC:** *Role-Based Access Control* — controle de acesso por papéis.

### 1.4 Referências

- Especificação de Objetivos e Requisitos (EOR) — Entrega 1 (referência histórica).
- Especificação de Requisitos de Software (ERS v2 — IEEE 830) — documento principal de requisitos.
- Levantamento de Requisitos consolidado — `docs/Levantamento_Requisitos_Clinica_Psicologia_Final.pdf`.
- Estudo do sistema análogo Agenday — `docs/01-estudo-sistema-analogo/`.
- ISO/IEC 25010:2011 — Modelo de qualidade de software.
- IEEE 830-1998 — *Recommended Practice for Software Requirements Specifications*.
- Resolução CFP nº 11/2018 — guarda e sigilo do prontuário psicológico.

### 1.5 Visão geral do documento

Este documento está estruturado em oito seções: (2) Posicionamento; (3) Descrição dos
stakeholders e usuários; (4) Visão geral do produto; (5) Funcionalidades do produto;
(6) Restrições, premissas e dependências; (7) Evolução futura e regras de negócio;
(8) Faixas de qualidade; além do **Anexo A — Mapeamento Necessidade ↔ Funcionalidade**.

---

## 2. Posicionamento

### 2.1 Oportunidade de negócio

Pequenas clínicas de psicologia e profissionais autônomos hoje gerenciam agenda,
prontuário e comunicação com pacientes por meio de **planilhas, mensageiros pessoais
(WhatsApp) e cadernos físicos**. Essa fragmentação produz dupla agenda, esquecimento de
sessões, falhas no acompanhamento clínico e insegurança jurídica (LGPD). Há demanda
concreta por uma ferramenta integrada, conforme apurado em entrevistas com Karla Niana
(proprietária de clínica) e Marla Gonçalves (psicóloga autônoma).

### 2.2 Definição do problema

| Aspecto | Descrição |
|---------|-----------|
| **O problema de** | descentralização de agenda, prontuário e comunicação em ferramentas separadas |
| **afeta** | proprietários de clínica, psicólogos autônomos, secretárias e pacientes |
| **cujo impacto é** | sessões esquecidas, retrabalho administrativo, insegurança jurídica (LGPD) e falta de acompanhamento clínico |
| **uma solução de sucesso seria** | uma plataforma única que integre agendamento, prontuário e comunicação automatizada, acessível em mobile, com controle de acesso por papel e aderente à LGPD |

### 2.3 Posicionamento do produto

Para **clínicas de psicologia de pequeno porte e psicólogos autônomos** que precisam
organizar agendamento, prontuário e comunicação em um único lugar, o **Sistema de Gestão
para Clínica de Psicologia** é uma plataforma web responsiva (mobile-first) de gestão
clínica integrada que centraliza agenda, prontuário eletrônico e lembretes automáticos
com aderência à LGPD. Diferentemente de soluções como o Agenday e de planilhas avulsas —
que cobram por usuário ou não oferecem prontuário/auditoria —, o produto combina
escalabilidade para clínicas multidisciplinares com simplicidade para autônomos, com
prontuário criptografado e auditoria de acesso.

---

## 3. Descrição dos stakeholders e usuários

### 3.1 Sumário dos stakeholders

| Stakeholder | Descrição | Responsabilidades / Interesse |
|-------------|-----------|-------------------------------|
| Karla Niana | Proprietária de clínica multidisciplinar | Define regras de negócio e fluxos operacionais |
| Marla Gonçalves | Psicóloga autônoma | Representa a persona "single user" mobile-first |
| Carol | Secretária de clínica | Identifica gargalos operacionais |
| Ana Julia Monteiro | Paciente | Preferências de agendamento e comunicação |
| **Conselho Federal de Psicologia (CFP)** | **Órgão regulador externo** | **Define normas de guarda, sigilo e acesso ao prontuário (Resolução CFP nº 11/2018); o sistema deve aderir a elas** |
| Equipe de desenvolvimento | Grupo da disciplina | Construção do sistema |
| Professor da disciplina | Avaliador acadêmico | Critérios de avaliação |

### 3.2 Sumário dos usuários

| Usuário | Descrição | Stakeholder representado |
|---------|-----------|-------------------------|
| Administrador da Clínica | Gerencia profissionais, especialidades e regras | Karla |
| Profissional (Psicólogo) | Atende pacientes e mantém prontuário | Karla, Marla |
| Secretária | Agenda consultas e envia lembretes | Carol |
| Paciente | Agenda (autoatendimento), recebe lembretes e acessa documentos | Ana Julia |

### 3.3 Ambiente do usuário

O sistema é uma **aplicação web responsiva (mobile-first)** acessada pelo navegador.
Hoje os usuários trabalham assim:

- **Secretária e administrador** operam em geral via **desktop**, em jornadas contínuas.
- **Profissionais e pacientes** acessam o sistema com mais frequência em **dispositivos
  móveis (navegador do celular)**, em sessões curtas entre atendimentos — o que exige uma
  **interface leve e rápida**.
- **Conectividade:** internet doméstica/comercial, eventualmente 4G em deslocamento.

> O acesso por celular é feito pelo **navegador** (interface responsiva). Um **aplicativo
> móvel nativo não faz parte desta entrega** — está previsto como **evolução futura** (§7.1).

### 3.4 Principais necessidades dos stakeholders e usuários

| ID | Necessidade | Prioridade | Solução atual | Solução proposta |
|----|-------------|-----------|---------------|------------------|
| N01 | Agenda unificada | Alta | Google Calendar + WhatsApp | Agenda integrada com lembretes automáticos |
| N02 | Prontuário acessível e seguro | Alta | Caderno físico / Word | Prontuário criptografado com auditoria |
| N03 | Lembretes automáticos | Alta | Mensagem manual no WhatsApp | Envio automático via WhatsApp/Email |
| N05 | Conformidade LGPD | Alta | Inexistente | Consentimento, criptografia e log de auditoria |
| N06 | Acesso mobile | Alta | Parcial | Web responsivo mobile-first |
| N07 | Controle de acesso por papel | Média | Inexistente | RBAC (Admin / Profissional / Secretária / Paciente) |
| ~~N04~~ | ~~Controle financeiro~~ | — | Planilha Excel | **Adiado — ver Evolução futura (§7.1)** |

---

## 4. Visão geral do produto

### 4.1 Perspectiva do produto

O Sistema de Gestão para Clínica de Psicologia é uma **plataforma web centralizada** cujas
**funcionalidades centrais** são: **agendamento de consultas** (incluindo autoagendamento
pelo paciente), **prontuário eletrônico** (anamnese e evolução por sessão), **registro de
atendimento** (presença e duração) e **comunicação automatizada** (lembretes por
WhatsApp/E-mail). Para a comunicação, o sistema **integra-se a serviços externos de
mensageria** (WhatsApp Business API e SMTP). Diferentemente de sistemas de Prontuário
Eletrônico hospitalar (PEP) de grande porte, o produto é dimensionado especificamente para
psicólogos autônomos e clínicas de pequeno porte, priorizando a agilidade do atendimento e
a segurança dos dados sensíveis conforme a LGPD.

### 4.2 Sumário das capacidades

| Benefício para o cliente | Funcionalidade de suporte |
|--------------------------|---------------------------|
| Reduzir faltas de pacientes | Lembretes automáticos com confirmação |
| Centralizar histórico clínico | Prontuário eletrônico com timeline de sessões |
| Conformidade LGPD | Consentimento explícito + criptografia + auditoria |
| Escalar de autônomo para clínica | RBAC + multi-profissional sem migração de dados |
| Atendimento ágil em mobilidade | Interface responsiva mobile-first |

> **Nota de terminologia.** Este documento descreve **capacidades/funcionalidades** do
> produto, e não "módulos". O *particionamento* do sistema em módulos é uma decisão de
> **arquitetura**, tomada em etapa posterior.

> **Diagrama de contexto.** O **Diagrama de Casos de Uso** (ver `docs/06-modelagem/02-casos-de-uso/`)
> funciona como diagrama de contexto: apresenta o sistema, os atores humanos (Paciente,
> Profissional, Secretária, Administrador) e os **sistemas externos** com que ele se
> relaciona (mensageria WhatsApp/E-mail, geração de PDF e auditoria).

### 4.3 Suposições e dependências

- Cliente possui acesso a internet estável.
- **WhatsApp Business API** disponível, ou alternativa contratada (**Z-API** ou **Twilio**).
- **Provedor SMTP** para e-mail (ex.: **SendGrid** ou **Mailgun**).
- Pacientes possuem e-mail e/ou telefone com WhatsApp.
- Hospedagem em provedor cloud com data center em território brasileiro (alinhamento LGPD).

### 4.4 Custos e preços

Não aplicável no momento, por se tratar de projeto desenvolvido em contexto acadêmico.

### 4.5 Licenciamento e instalação

O sistema é uma aplicação web acessada diretamente pelo navegador, não exigindo instalação
nos dispositivos da clínica ou dos pacientes. O código-fonte e o histórico de versões são
mantidos em repositório no GitHub.

---

## 5. Funcionalidades do produto

| ID | Funcionalidade | Descrição | Prioridade |
|----|----------------|-----------|-----------|
| F01 | Cadastro de profissionais | Registro com CRP, especialidade e agenda padrão | Alta |
| F02 | Cadastro de pacientes | Dados pessoais, contatos, consentimento LGPD, vínculo com responsável legal | Alta |
| F03 | Agendamento de consulta | Marcar, remarcar e cancelar com regras de antecedência; autoagendamento pelo paciente; recorrência; sessões sequenciais no mesmo dia | Alta |
| F04 | Lembretes automáticos | Envio de avisos prévios e processamento da resposta (confirmação/cancelamento) | Alta |
| F05 | Prontuário eletrônico | Anamnese, evolução por sessão e anexos | Alta |
| F06 | Registro de atendimento | Marcar presença/falta, duração e observações | Alta |
| F09 | Relatórios operacionais | Ocupação de agenda, faltas e cancelamentos | Média |
| F10 | Controle de acesso (RBAC) | Papéis Admin / Profissional / Secretária / Paciente | Alta |
| F11 | Auditoria e log | Registro de quem acessou o quê e quando | Alta |
| F12 | Consentimento LGPD | Aceite explícito + revogação a qualquer momento | Alta |
| F13 | Portal do paciente | Solicitar agendamentos, visualizar histórico de sessões e cancelar | Média |

**Funcionalidades adiadas (ver §7.1):** ~~F07 Emissão de recibo~~ · ~~F08 Controle financeiro
e Dashboard~~ · relatórios financeiros.

---

## 6. Restrições, premissas e dependências

### 6.1 Restrições

Restrição = requisito que **não admite alternativa de implementação**.

- **R01:** O sistema **deve** estar em conformidade com a **LGPD** (Lei 13.709/2018).
- **R02:** O sistema **deve** aderir à **Resolução CFP nº 11/2018** quanto à guarda e ao
  sigilo do prontuário.
- **R03:** O sistema **deve** ser desenvolvido como **aplicação Web responsiva**.
- **R04:** Tecnologias **devem** ser open-source ou ter licença educacional.
- **R05:** Toda comunicação **deve** trafegar sobre **HTTPS** (certificado SSL obrigatório).
- **R06:** Prazo de entrega definido pelo cronograma da disciplina.

### 6.2 Premissas

- **P01:** Equipe de 5 integrantes com cargas equivalentes.
- **P02:** Stakeholders entrevistados (Karla, Marla) seguem disponíveis para validação.
- **P03:** A API do WhatsApp Business é viável ou substituível por provedor compatível.

### 6.3 Dependências

- **D01:** Provedor de e-mail SMTP (ex.: SendGrid, Mailgun).
- **D02:** Provedor de WhatsApp Business API (Twilio, Z-API ou Meta direta).
- **D03:** Certificado SSL para HTTPS.
- **D04:** Provedor de infraestrutura em nuvem para hospedagem da aplicação e do banco.

---

## 7. Evolução futura e regras de negócio

### 7.1 Requisitos / Evolução futura

Itens **fora do escopo desta entrega**, previstos para versões posteriores (não são
restrições):

- **EF01 — Módulo financeiro:** cobrança, registro de pagamento (dinheiro, PIX, cartão,
  plano), emissão de recibo com dados fiscais, controle de inadimplência e relatórios
  financeiros. *(Depende de integração com gateway de pagamento, ex.: Mercado Pago.)*
- **EF02 — Aplicativo móvel nativo:** evolução da interface responsiva para app nativo
  (Android/iOS).
- **EF03 — Integração com Google Calendar.**

### 7.2 Regras de negócio

Regras do domínio que o sistema deve respeitar (não são restrições técnicas nem
funcionalidades):

- **RN01:** Apenas o **profissional responsável** pode acessar o prontuário de um paciente
  (sigilo CFP); o acesso multidisciplinar limita-se a um **resumo/acompanhamento**.
- **RN02:** Nenhum **laudo** é liberado ao paciente sem **aprovação do profissional**.
- **RN03:** A sessão de **anamnese** precede o acompanhamento; ao registrar evolução, o
  paciente já está cadastrado e vinculado ao profissional.

---

## 8. Faixas de qualidade

Os requisitos não funcionais (RNFs) detalhados estão na **Especificação Suplementar**
(ISO/IEC 25010). Resumo das faixas-alvo (todas verificáveis objetivamente):

| Característica ISO | Alvo |
|--------------------|------|
| Adequação funcional | 100% dos RFs de prioridade Alta cobertos por testes de aceitação |
| Desempenho | Resposta < 2 s em 95% das requisições de leitura |
| Compatibilidade | Navegadores Chrome, Edge, Safari e Firefox; responsivo de 320 px a 1920 px |
| Usabilidade | Agendamento pelo paciente em < 2 min; onboarding de profissional/secretária em < 10 min sem treinamento |
| Confiabilidade | Disponibilidade ≥ 99% mensal; backup diário com retenção de 30 dias e restore atestado |
| Segurança | Criptografia em trânsito (TLS 1.2+) e em repouso (AES-256); trilha de auditoria; timeout de sessão por inatividade |
| Manutenibilidade | Cobertura de testes ≥ 70% no backend |
| Portabilidade | Stack containerizada (Docker); migração entre provedores cloud sem alteração de código |

---

## Anexo A — Mapeamento Necessidade ↔ Funcionalidade

| Necessidade | Funcionalidade |
|-------------|----------------|
| N01 — Agenda unificada | F01, F03, F04, F13 |
| N02 — Prontuário seguro | F02, F05, F06, F11, F12 |
| N03 — Lembretes automáticos | F04 |
| N05 — Conformidade LGPD | F11, F12 |
| N06 — Acesso mobile | (não-funcional — ver Especificação Suplementar) |
| N07 — Controle de acesso | F10 |
| ~~N04 — Controle financeiro~~ | ~~F07, F08, F09~~ → **adiado (EF01)**; F09 (relatórios operacionais) permanece |

# UC01 — Agendar Consulta

**Autor:** Equipe (revisão da Entrega 2)
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 6 (formato *fully dressed*, caso de uso essencial)

---

## Identificação

| Campo | Valor |
|---|---|
| **ID** | UC01 |
| **Nome** | Agendar Consulta |
| **Escopo** | Sistema de Gestão para Clínica de Psicologia |
| **Nível** | Objetivo do usuário |
| **Ator Principal** | **Paciente** (autoagendamento) — é quem mais tem interesse em marcar a consulta |
| **Atores de apoio** | Secretária (pode agendar em nome do paciente), Profissional (define agenda e regras) |
| **Atores Secundários (sistemas)** | Sistema de mensageria (lembrete), Sistema de Auditoria |
| **Frequência** | Alta |

> **Por que o ator principal é o Paciente?** Um caso de uso descreve quem tem o objetivo.
> Quem deseja a consulta é o paciente; o profissional vem com o trabalho dele e a secretária
> opera o balcão. Com autoagendamento, o próprio paciente realiza a marcação. A secretária
> faz a mesma coisa "em nome do paciente" — por isso ela aparece como **ator de apoio** e o
> fluxo dela é tratado como **fluxo alternativo**, não como um caso de uso separado.

## Stakeholders e Interesses

| Stakeholder | Interesse |
|---|---|
| Paciente | Marcar um horário disponível de forma rápida, sem ficar trocando mensagens |
| Profissional | Ter a agenda preenchida sem conflito; controlar regras (antecedência, recorrência) |
| Secretária | Conseguir marcar pelo paciente quando ele liga/aparece na clínica |
| Clínica | Evitar choque de sala/horário; reduzir no-show com lembrete automático |

## Pré-condições

- PRE01 — Paciente cadastrado e autenticado (ou identificado pela secretária).
- PRE02 — Existe ao menos um profissional com **agenda configurada** (horários e duração de slot).
- PRE03 — Consentimento LGPD do paciente vigente.

## Pós-condições

- POS01 — Agendamento criado com status AGENDADO, vinculando **paciente + profissional + sala + horário**.
- POS02 — O horário (slot), a sala e a agenda do profissional ficam **reservados** (sem dupla marcação).
- POS03 — Lembrete(s) programado(s) para o agendamento (ver [UC04](UC04_Enviar_Lembrete.md)).
- POS04 — Log de auditoria registra a criação do agendamento.

---

## Cenário Principal de Sucesso — Paciente faz o autoagendamento

> Caso de uso **essencial**: descreve a interação (ator ↔ sistema) sem telas/botões, em
> passos de granularidade homogênea. Não há decisão condicional no fluxo principal — os
> desvios ("ou… ou…") estão nos fluxos alternativos/exceção.

1. O **Paciente** solicita agendar uma consulta.
2. O **sistema** solicita o profissional (ou a especialidade) desejado.
3. O **Paciente** informa o profissional/especialidade.
4. O **sistema** solicita o período desejado (data aproximada / faixa de horário).
5. O **Paciente** informa o período desejado.
6. O **sistema** **verifica a disponibilidade conjunta** de profissional, sala e paciente
   no período e apresenta os horários livres (considerando a agenda do profissional, a
   ocupação das salas e os bloqueios existentes).
7. O **Paciente** escolhe um horário.
8. O **sistema** confirma a reserva do horário (profissional + sala + paciente fecham juntos)
   e cria o agendamento.
9. O **sistema** programa o lembrete e informa ao Paciente a confirmação do agendamento.

> **Disponibilidade conjunta (passo 6).** O horário só é ofertado se **profissional, sala e
> paciente** estiverem livres ao mesmo tempo — os três precisam "fechar juntos", senão não há
> como confirmar. A clínica tem poucas salas (ex.: 3 + 1 multidisciplinar), então a alocação
> de sala é um ponto sensível: o sistema checa o conflito de sala antes de ofertar o slot.

## Fluxos alternativos

### 3-9a. Secretária agenda em nome do paciente
- 3a1. A **Secretária** seleciona/identifica o paciente (busca por nome/CPF) e segue do passo 4.
  O restante do fluxo é idêntico, apenas com a secretária operando pelo paciente.

### 6a. Recorrência (tratamento contínuo)
- 6a1. O **Paciente/Secretária** indica que a consulta é **recorrente** e informa a regra:
  frequência (ex.: toda quinta às 14h) e **de quando até quando** (data fim ou nº de sessões;
  pode ser indeterminado, renovável).
- 6a2. O **sistema** gera a **série de agendamentos** no período informado, reservando o
  mesmo horário/sala a cada ocorrência, e sinaliza as datas em que houver conflito para
  ajuste manual.

### 6b. Sessões sequenciais no mesmo dia (atendimento multidisciplinar)
- 6b1. O **Paciente** precisa de mais de um atendimento no mesmo dia (ex.: terapia + fono),
  para evitar ir e voltar à clínica várias vezes na semana — situação comum em pacientes com
  limitações de deslocamento.
- 6b2. O **Paciente/Secretária** solicita **sessões sequenciais no mesmo dia**.
- 6b3. O **sistema** prioriza horários **adjacentes** entre as especialidades, verificando a
  disponibilidade conjunta de cada profissional e sala envolvidos.

## Fluxos de exceção

### 7e. Horário escolhido ficou indisponível (concorrência)
- 7e1. Outro agendamento tomou o slot nesse meio-tempo.
- 7e2. O **sistema** informa que o horário não está mais disponível e retorna ao passo 6
  com a lista atualizada.

### 8e. Conflito de sala / profissional / paciente na confirmação
- 8e1. O **sistema** detecta que um dos três (sala, profissional ou paciente) ficou ocupado.
- 8e2. O **sistema** não confirma o agendamento e apresenta os horários alternativos mais próximos.

### *e. Sem horários disponíveis no período
- \*e1. O **sistema** informa a ausência de horários e sugere o período livre mais próximo
  ou uma lista de espera.

---

## Requisitos especiais

- RE01 — A oferta de horários (passo 6) deve responder em ≤ 2s (RNF-PE-01, transversal).
- RE02 — A reserva do slot deve ser **atômica** para impedir dupla marcação (controle de concorrência).
- RE03 — Toda criação/alteração de agendamento é registrada em auditoria (RNF-SE-06, transversal).
- RE04 — Regras de agenda (antecedência, recorrência, autoagendamento por profissional) são
  **parametrizáveis** — o sistema não é exclusivo de uma clínica.

## Lista de tecnologia e variações de dados

- Modalidade: { PRESENCIAL, ONLINE }.
- Tipo de sessão: { ANAMNESE, RETORNO, AVALIACAO, GRUPAL }.
- Status do agendamento: { AGENDADO, CONFIRMADO, REALIZADO, CANCELADO, FALTOU }.
- Recorrência: { NENHUMA, SEMANAL, QUINZENAL, MENSAL } + (data fim | nº de sessões | indeterminada).

## Regras de Negócio

- **RN01** — Não pode haver dupla marcação para o mesmo profissional, a mesma sala ou o mesmo
  paciente no mesmo intervalo.
- **RN02** — O autoagendamento pelo paciente só está disponível se o profissional o habilitar
  (`permite_autoagendamento`).
- **RN03** — Cancelamento/remarcação respeita a antecedência mínima configurada (tratado em
  **UC_Cancelar_Remarcar**).
- **RN04** — A duração do slot é parametrizável por profissional (ex.: 40, 50 ou 60 min;
  avaliação pode levar 2 horas).

## Questões em aberto

- Q01 — Lista de espera: notificar automaticamente quando vagar um horário?
- Q02 — Limite de quantas sessões futuras um paciente pode ter agendadas simultaneamente?

## Rastreabilidade

- Atende RFs: RF06, RF07, RF08, RF09, RF10 (agendamento, prevenção de conflito, autoagendamento, recorrência).
- Atende necessidades: N01 (Agenda unificada).
- Atende RNFs: RNF-PE-01, RNF-SE-06.
- Casos de uso relacionados: [UC04 Enviar Lembrete](UC04_Enviar_Lembrete.md) (`<<include>>`),
  UC_Cancelar_Remarcar, UC_Consentimento_LGPD (`<<include>>`).
- Telas relacionadas: `Tela_Marcar_Consulta` (M03), `Tela_Agenda_Diaria` (M07).
- Entidades envolvidas: Paciente, Profissional, AgendaConfig, Agendamento, (Sala — ver MER), Lembrete.

# UC03 — Registrar Atendimento

**Autor:** Equipe (revisão da Entrega 2)
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 6 (formato *fully dressed*)

---

> **Nota de revisão (escopo e granularidade).** O caso de uso original "Registrar
> Atendimento **e Recibo**" misturava duas coisas distintas: *registrar o atendimento*
> (presença, duração) e *receber pagamento / emitir recibo*. Conforme orientação, foram
> **separados** — um caso de uso não deve "fazer isso **e** aquilo". Aqui fica apenas o
> **registro do atendimento**; a parte de **pagamento e recibo** vai para o módulo
> financeiro (evolução futura, EF01). Além disso, o antigo "Registrar Presença" (UC07) era
> redundante com este — foi **fundido** aqui, pois registrar presença/falta é parte de
> registrar o atendimento.

## Identificação

| Campo | Valor |
|---|---|
| **ID** | UC03 |
| **Nome** | Registrar Atendimento |
| **Escopo** | Sistema de Gestão para Clínica de Psicologia |
| **Nível** | Objetivo do usuário |
| **Ator Principal** | Profissional (psicólogo) — registra o atendimento que realizou |
| **Atores de apoio** | Secretária (pode registrar presença/falta) |
| **Atores Secundários** | Sistema de Auditoria |
| **Frequência** | Alta — uma vez por sessão |

## Stakeholders e Interesses

| Stakeholder | Interesse |
|---|---|
| Profissional | Marcar que o atendimento ocorreu e registrar a evolução em seguida |
| Secretária | Registrar presença/falta para controle da agenda |
| Clínica | Ter dados de presença/falta confiáveis para relatórios de ocupação e no-show |

## Pré-condições

- PRE01 — Existe um Agendamento com status AGENDADO ou CONFIRMADO para a data.
- PRE02 — Usuário autenticado com permissão (Profissional ou Secretária).

## Pós-condições

- POS01 — Atendimento registrado com **presença/falta**, **duração** e **observações**.
- POS02 — Status do Agendamento atualizado para REALIZADO (presente) ou FALTOU (ausente).
- POS03 — Log de auditoria registra o evento.

---

## Cenário Principal de Sucesso — Registrar presença

1. O **Profissional** seleciona o atendimento do dia a registrar.
2. O **sistema** apresenta os dados do agendamento (paciente, horário, modalidade).
3. O **Profissional** confirma a **presença** do paciente e informa a **duração** e
   **observações** do atendimento.
4. O **sistema** registra o atendimento, atualiza o Agendamento para REALIZADO e registra auditoria.
5. O **sistema** confirma o registro e oferece registrar a **evolução** do prontuário
   (segue para [UC02 Manter Prontuário](UC02_Manter_Prontuario.md), via `<<extend>>`).

## Fluxos alternativos

### 3a. Paciente faltou
- 3a1. O **Profissional/Secretária** marca **falta**.
- 3a2. O **sistema** atualiza o Agendamento para FALTOU e registra auditoria (alimenta o
  relatório de no-shows — [UC05](UC05_Gerar_Relatorio.md)).

### 1b. Secretária registra a presença
- 1b1. A **Secretária** registra apenas presença/falta e duração; a evolução clínica fica a
  cargo do profissional.

## Fluxos de exceção

### 2e. Não há agendamento correspondente
- 2e1. O **sistema** informa que não há agendamento e oferece criar um atendimento avulso
  (encaixe), seguindo as regras de disponibilidade do [UC01](UC01_Agendar_Consulta.md).

---

## Requisitos especiais

- RE01 — Registro de atendimento concluído em ≤ 2s (RNF-PE-01, transversal).
- RE02 — Evento registrado em auditoria (RNF-SE-06, transversal).

## Lista de tecnologia e variações de dados

- Presença: { PRESENTE, FALTOU }.
- Duração: em minutos (default = duração do slot do profissional).

## Regras de Negócio

- **RN01** — Só é possível registrar atendimento para agendamentos do dia ou passados (não futuros).
- **RN02** — Uma falta marcada alimenta a contagem de no-show do paciente.
- **RN03** — O registro de presença é pré-requisito para registrar a evolução clínica.

## Questões em aberto

- Q01 — Permitir "atendimento avulso" (sem agendamento prévio) ou exigir sempre o agendamento?

## Rastreabilidade

- Atende RFs: RF15 (registro de atendimento — presença/falta, duração, observações).
- Atende necessidades: N02 (Prontuário/atendimento), N01 (dados para ocupação/no-show).
- Atende RNFs: RNF-PE-01, RNF-SE-06.
- Casos de uso relacionados: [UC02 Manter Prontuário](UC02_Manter_Prontuario.md) (`<<extend>>`),
  [UC05 Gerar Relatório](UC05_Gerar_Relatorio.md) (consome dados de presença/falta).
- Telas relacionadas: `Tela_Iniciar_Sessao` (M08), `Tela_Encerrar_Sessao` (M10).
- Entidades envolvidas: Agendamento, Atendimento, Profissional, Paciente, LogAuditoria.
- *Pagamento/Recibo: ver evolução futura (EF01).*

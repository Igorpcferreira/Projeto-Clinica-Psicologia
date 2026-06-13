# Casos de Uso

**Responsavel:** Pedro (diagrama); 1 UC detalhado por integrante
**Referencia:** Larman cap. 6 (formato *fully dressed*, casos de uso essenciais)
**Status:** Atualizado na revisao da Entrega 2 (escopo reduzido — foco em agendamento/atendimento)

## Diagrama
- [`Diagrama_Casos_Uso.puml`](Diagrama_Casos_Uso.puml) — fonte (reexportar `.png`/`.svg`).
  Serve tambem como **diagrama de contexto** (sistema + atores + sistemas externos).

## Casos de uso detalhados (no escopo desta entrega)
| ID | Nome | Ator principal | Arquivo |
|----|------|----------------|---------|
| UC01 | Agendar Consulta | Paciente (autoagendamento) | [UC01_Agendar_Consulta.md](UC01_Agendar_Consulta.md) |
| UC_Anamnese | Realizar Anamnese | Profissional | [UC_Anamnese.md](UC_Anamnese.md) |
| UC02 | Manter Prontuário | Profissional | [UC02_Manter_Prontuario.md](UC02_Manter_Prontuario.md) |
| UC03 | Registrar Atendimento | Profissional | [UC03_Registrar_Atendimento.md](UC03_Registrar_Atendimento.md) |
| UC04 | Enviar Lembrete | Sistema (agendador interno) | [UC04_Enviar_Lembrete.md](UC04_Enviar_Lembrete.md) |
| UC05 | Gerar Relatório Operacional | Administrador | [UC05_Gerar_Relatorio.md](UC05_Gerar_Relatorio.md) |

## Mudancas aplicadas (feedback da professora)
- **UC01** criado: ator principal **Paciente** (maior interessado); passos com granularidade
  homogenea; **disponibilidade conjunta** de profissional+sala+paciente; **recorrencia** e
  **sessoes sequenciais no mesmo dia**; sem condicional no fluxo principal.
- **UC_Anamnese** separado do prontuario (abre o prontuario e cria o vinculo).
- **UC03** = so "Registrar Atendimento" (presenca/falta/duracao). O antigo "Registrar
  Atendimento **e Recibo**" foi dividido e o "Registrar Presenca" (UC07) foi **fundido** aqui.
- **UC04**: esclarecido que o agendador e **interno** (classe ativa / padrao Observer).
- **UC05**: so relatorios **operacionais**, com dados de cada relatorio especificados.
- **Financeiro** (recibo, pagamento, dashboard, relatorios financeiros) → **evolucao futura**.

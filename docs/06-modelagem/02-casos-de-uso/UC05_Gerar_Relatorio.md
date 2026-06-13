# UC05 — Gerar Relatório Operacional

**Autor:** Pedro Lourençoni Lima
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 6 (formato *fully dressed*)

---

> **Escopo desta entrega.** "Gerar relatório" é genérico demais para um único caso de uso —
> cada relatório tem **estrutura e dados próprios**. Por isso, este UC trata apenas os
> **relatórios operacionais** (cada um com seus dados especificados abaixo): **Ocupação de
> Agenda**, **No-shows e Remarcações** e **Atendimentos por Especialidade**. Os **relatórios
> financeiros** (Faturamento, Comissão, Recebíveis/Inadimplência, carnê-leão) ficam para a
> **evolução futura** junto do módulo financeiro (ver Documento de Visão §7.1) e estão
> listados ao final apenas como referência.

## Identificação

| Campo | Valor |
|---|---|
| **ID** | UC05 |
| **Nome** | Gerar Relatório Operacional |
| **Escopo** | Sistema de Gestão para Clínica de Psicologia |
| **Nível** | Objetivo do usuário |
| **Ator Principal** | Administrador (maior interessado na visão da operação); Profissional acessa visão restrita aos próprios dados |
| **Atores Secundários** | Sistema de Geração de PDF, Sistema de Geração de CSV |
| **Frequência** | Média — fim de mês, fim de trimestre, sob demanda |

## Stakeholders e Interesses

| Stakeholder | Interesse |
|---|---|
| Administrador | Visibilidade da operação (ocupação de agenda, no-shows, atendimentos por especialidade) |
| Profissional | Acompanhar a própria ocupação e taxa de faltas |
| Secretária | Planejar a agenda a partir de faltas e remarcações |
| Auditor | Capacidade de exportar evidências para auditoria |

## Pré-condições

- PRE01 — Usuário autenticado.
- PRE02 — Existe ao menos um agendamento/atendimento no período solicitado.
- PRE03 — Permissão conforme RBAC: Admin vê tudo da clínica; Profissional vê só seus próprios dados.

## Pós-condições

- POS01 — Relatório renderizado em tela com filtros aplicados.
- POS02 — Se exportado, arquivo (PDF/CSV) gerado e armazenado para download.
- POS03 — Log de auditoria registra a consulta e qualquer exportação.

---

## Cenário Principal de Sucesso — Relatório de Ocupação

1. O **ator** acessa a área de "Relatórios".
2. O **sistema** apresenta os tipos operacionais disponíveis: { Ocupação de Agenda, No-shows e Remarcações, Atendimentos por Especialidade }.
3. O **ator** seleciona "Ocupação de Agenda".
4. O **sistema** apresenta filtros: período (data inicial/final, default últimos 30 dias), profissional (default: Todos para Admin / Eu para Profissional), tipo de sessão.
5. O **ator** define os filtros e aciona "Gerar".
6. O **sistema** consulta os dados respeitando RBAC.
7. O **sistema** apresenta:
   - Gráfico de barras de ocupação por profissional (% de slots realizados sobre slots disponíveis).
   - Tabela detalhada (profissional, slots disponíveis, agendados, realizados, no-shows, ocupação%).
   - KPIs no topo: ocupação média, total realizado, total no-show.
8. O **ator** opcionalmente aciona "Exportar PDF" ou "Exportar CSV".
9. O **sistema** gera o arquivo, registra log de auditoria com tipo, filtros, ator, e disponibiliza para download.

## Extensões

### 6a. Período sem dados
6a1. O **sistema** mostra empty state com sugestão de ajustar filtros.

### 6b. Período muito grande (> 12 meses)
6b1. O **sistema** alerta sobre tempo de geração e oferece "Continuar" ou "Ajustar período".

### 6c. Profissional tenta ver dados de outro profissional
6c1. O **sistema** filtra automaticamente para os próprios dados do Profissional, sem exibir aviso (a menos que ele tenta exportar com filtro explícito de outro profissional → 403).

### 9a. Falha na geração do PDF
9a1. O **sistema** mantém a tela renderizada e mostra "Falha ao gerar PDF — tente novamente" com retry.

### 9b. Arquivo CSV grande (> 50 MB)
9b1. O **sistema** comprime em ZIP e oferece download.

---

## Cenários adicionais (relatórios operacionais)

### Cenário B — Relatório de No-shows e Remarcações

1. Ator seleciona "No-shows e Remarcações".
2. Filtros: período, profissional, paciente.
3. Sistema lista pacientes ordenados por nº de no-shows desc, com colunas: **paciente,
   faltas, remarcações, taxa de no-show (%)**.
4. KPIs: total de faltas no período, taxa média de no-show da clínica.

### Cenário C — Relatório de Atendimentos por Especialidade

1. Ator seleciona "Atendimentos por Especialidade".
2. Filtros: período, especialidade.
3. Sistema apresenta, **por especialidade**: nº de atendimentos realizados, nº de pacientes
   distintos atendidos e participação (%) sobre o total da clínica (gráfico de barras + tabela).

---

## Relatórios financeiros — evolução futura (fora do escopo desta entrega)

Listados apenas para referência; serão especificados junto com o módulo financeiro (EF01):

- **Comissão por Profissional** — sessões realizadas, faturamento gerado, % e valor de comissão.
- **Faturamento Mensal** — receita bruta, recibos emitidos, recebido por forma de pagamento.
- **Recebíveis e Inadimplência** — pagamentos pendentes/inadimplentes por faixa de atraso.
- **Carnê-leão** — exportação anual no padrão da Receita Federal (G09 do VPC).

---

## Requisitos especiais

- RE01 — Geração de relatório mensal deve concluir em ≤ 5s (RNF-PE-04).
- RE02 — Filtros aplicados devem ser preservados ao recarregar a tela (deeplink).
- RE03 — Profissional vê apenas os próprios dados (RBAC); Admin vê toda a clínica.
- RE04 — Toda exportação registra log com filtros aplicados (LGPD — accountability).
- RE05 — PDF gerado em PDF/A-3 para arquivamento (RNF-CO-05).
- RE06 — CSV em UTF-8 com BOM para abrir no Excel sem problema de acentuação.

## Lista de tecnologia e variações de dados

- Tipos de relatório (escopo atual): { OCUPACAO, NO_SHOWS, ATENDIMENTOS_POR_ESPECIALIDADE }.
  Futuros (financeiro): { COMISSAO, FATURAMENTO, RECEBIVEIS, CARNE_LEAO }.
- Formatos de exportação: { PDF, CSV, ZIP (CSV grande) }.
- Granularidades de período: dia, semana, mês, trimestre, ano.
- Idioma: pt-BR.
- Encoding CSV: UTF-8 com BOM.

## Frequência de ocorrência

- Picos no início de cada mês (Admin) e em janeiro/fevereiro (relatório anual para IR — autônomos).
- Estimativa: ~10 a 30 gerações por mês em uma clínica.

## Regras de Negócio

- **RN01** — Profissional só pode ver dados próprios; Admin vê tudo da clínica.
- **RN02** — No-show conta apenas se Agendamento.status = FALTOU (não conta cancelamento prévio).
- **RN03** — Ocupação (%) = slots realizados ÷ slots disponíveis no período (por profissional).
- *(Regras de cálculo financeiro — comissão, faturamento, inadimplência, carnê-leão — ficam para o módulo financeiro futuro, EF01.)*

## Questões em aberto

- Q01 — Apresentar comparativo período-a-período (este mês vs. mês anterior)?
- Q02 — Permitir agendamento de envio automático do relatório por e-mail (ex.: todo dia 1º do mês)?

## Rastreabilidade

- Atende RFs: RF23 (Ocupação), RF24 (No-shows), RF25 (Exportação PDF/CSV).
- Atende necessidades: N01 (Agenda unificada — visibilidade da operação).
- Atende RNFs: RNF-PE-04 (geração ≤ 5s), RNF-CO-05 (PDF/A), RNF-SE-05 (RBAC), RNF-SE-06 (auditoria).
- Telas relacionadas: `Tela_Relatorios`.
- Entidades envolvidas: Agendamento, Atendimento, Profissional, Especialidade, AgendaConfig, LogAuditoria.
- *Relatórios financeiros (RF financeiro futuro) e entidades Pagamento/Recibo: ver evolução futura (EF01).*

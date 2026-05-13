# UC05 — Gerar Relatório Operacional/Financeiro

**Autor:** Pedro Lourençoni Lima
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 6 (formato *fully dressed*)

---

## Identificação

| Campo | Valor |
|---|---|
| **ID** | UC05 |
| **Nome** | Gerar Relatório Operacional/Financeiro |
| **Escopo** | Sistema de Gestão para Clínica de Psicologia |
| **Nível** | Objetivo do usuário |
| **Ator Principal** | Administrador (Karla) ou Profissional (Marla — visão restrita aos próprios dados) |
| **Atores Secundários** | Sistema de Geração de PDF, Sistema de Geração de CSV |
| **Frequência** | Média — fim de mês, fim de trimestre, sob demanda |

## Stakeholders e Interesses

| Stakeholder | Interesse |
|---|---|
| Administrador | Visibilidade da operação (ocupação, no-shows, faturamento, comissão) |
| Profissional autônomo | Controle do próprio negócio; relatório anual para imposto |
| Secretária | Saber quem está inadimplente; planejar cobrança |
| Profissional empregado | Relatório de comissão correto |
| Auditor | Capacidade de exportar evidências para auditoria |

## Pré-condições

- PRE01 — Usuário autenticado.
- PRE02 — Existe ao menos um agendamento/atendimento/pagamento no período solicitado.
- PRE03 — Permissão conforme RBAC: Admin vê tudo da clínica; Profissional vê só seus próprios dados.

## Pós-condições

- POS01 — Relatório renderizado em tela com filtros aplicados.
- POS02 — Se exportado, arquivo (PDF/CSV) gerado e armazenado para download.
- POS03 — Log de auditoria registra a consulta (especialmente para relatórios financeiros).

---

## Cenário Principal de Sucesso — Relatório de Ocupação

1. O **ator** acessa o módulo "Relatórios".
2. O **sistema** apresenta os tipos disponíveis: { Ocupação de Agenda, No-shows e Remarcações, Comissão por Profissional, Faturamento Mensal, Recebíveis e Inadimplência, Atendimentos por Especialidade }.
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

## Cenários adicionais

### Cenário B — Relatório de No-shows e Remarcações

1. Ator seleciona "No-shows e Remarcações".
2. Filtros: período, profissional, paciente.
3. Sistema lista pacientes ordenados por nº de no-shows desc, com colunas: paciente, faltas, remarcações, taxa de no-show.
4. Ação possível: "Aplicar política de no-show" (vide RN da política).

### Cenário C — Relatório de Comissão por Profissional

1. Ator (Admin) seleciona "Comissão por Profissional".
2. Filtros: período, profissional.
3. Sistema apresenta: profissional, sessões realizadas, faturamento gerado, % de comissão configurada, valor da comissão a pagar.
4. Exportação para folha de pagamento (CSV).

### Cenário D — Relatório de Faturamento Mensal (Marla — modo autônomo)

1. Marla acessa "Faturamento Mensal".
2. Sistema apresenta: receita bruta, recibos emitidos, inadimplência, recebido por forma de pagamento (gráfico pizza).
3. Botão "Exportar carnê-leão" → CSV no padrão da Receita Federal (G09 do VPC).

### Cenário E — Relatório de Recebíveis e Inadimplência

1. Ator (Admin/Secretária) seleciona "Recebíveis e Inadimplência".
2. Lista pagamentos com status PENDENTE e INADIMPLENTE.
3. Filtros: idade da dívida (até 7 dias, 8-30, 31-60, > 60).
4. Ação possível: "Disparar lembrete de pagamento" (UC_Lembrete_Pagamento).

---

## Requisitos especiais

- RE01 — Geração de relatório mensal deve concluir em ≤ 5s (RNF-PE-04).
- RE02 — Filtros aplicados devem ser preservados ao recarregar a tela (deeplink).
- RE03 — Relatórios financeiros (faturamento, comissão, recebíveis) ficam atrás de RBAC; Profissional vê só os seus dados.
- RE04 — Toda exportação registra log (RE-AT-EXPORT) com filtros aplicados (LGPD — accountability).
- RE05 — PDF gerado em PDF/A-3 para arquivamento (RNF-CO-05).
- RE06 — CSV em UTF-8 com BOM para abrir no Excel sem problema de acentuação.

## Lista de tecnologia e variações de dados

- Tipos de relatório: { OCUPACAO, NO_SHOWS, COMISSAO, FATURAMENTO, RECEBIVEIS, ATENDIMENTOS_POR_ESPECIALIDADE }.
- Formatos de exportação: { PDF, CSV, ZIP (CSV grande) }.
- Granularidades de período: dia, semana, mês, trimestre, ano.
- Idioma: pt-BR.
- Encoding CSV: UTF-8 com BOM.

## Frequência de ocorrência

- Picos no início de cada mês (Admin) e em janeiro/fevereiro (relatório anual para IR — autônomos).
- Estimativa: ~10 a 30 gerações por mês em uma clínica.

## Regras de Negócio

- **RN01** — Profissional só pode ver dados próprios; Admin vê tudo da clínica.
- **RN02** — Comissões são calculadas a partir de Pagamento.status = PAGO no período.
- **RN03** — No-show conta apenas se Agendamento.status = FALTOU (não conta cancelamento prévio).
- **RN04** — Faturamento bruto = soma de Pagamento.valor onde status = PAGO no período.
- **RN05** — Inadimplência = Pagamento.status = INADIMPLENTE OU (status = PENDENTE AND vencimento < hoje).
- **RN06** — Relatório anual de carnê-leão filtra apenas pagamentos com forma ≠ PLANO (planos de saúde têm tributação distinta).

## Questões em aberto

- Q01 — Calcular comissão por meta atingida (ex.: % maior se passar de X sessões/mês)?
- Q02 — Apresentar comparativo período-a-período (este mês vs. mês anterior)?
- Q03 — Permitir agendamento de envio automático do relatório por e-mail (ex.: todo dia 1º do mês)?
- Q04 — Inclusão de impostos retidos (PIS/COFINS, ISS) no faturamento líquido — está no escopo?

## Rastreabilidade

- Atende RFs: RF21 (Dashboard), RF22 (Comissão), RF23 (Ocupação), RF24 (No-shows), RF25 (Exportação).
- Atende necessidades: N04 (Controle financeiro).
- Atende RNFs: RNF-PE-04 (geração ≤ 5s), RNF-CO-05 (PDF/A), RNF-SE-05 (RBAC), RNF-SE-06 (auditoria).
- Telas relacionadas: `Tela_Relatorios`, `Tela_Dashboard_Financeiro`.
- Entidades envolvidas: Agendamento, Atendimento, Pagamento, Recibo, Profissional, Especialidade, AgendaConfig, LogAuditoria.

# Value Proposition Canvas — Papel: Psicólogo Autônomo (persona Marla Gonçalves)

**Papel representado:** **Psicólogo autônomo** — Marla Gonçalves é a persona concreta
usada para ilustrar o papel; o canvas vale para o **perfil**, não só para ela.
**Autor:** Pedro Lourençoni Lima
**Referência:** *Testing Business Ideas* (Strategyzer), pgs. 38-39
**Insumos:** entrevista da Entrega 1 conduzida por Pedro (`docs/02-entrevistas/Entrevista-Marla.docx.pdf`)

> **Escopo da Entrega 2.** O VPC mantém a visão completa do valor para o papel, mas os
> itens ligados ao **financeiro** (cobrança PIX, recibo, resumo financeiro, carnê-leão)
> estão marcados com **(futuro)** porque o módulo financeiro foi adiado nesta entrega
> (ver Documento de Visão §7.1).

---

## Contexto da persona

| Atributo | Valor |
|---|---|
| Nome | Marla Gonçalves |
| Papel | Psicóloga autônoma |
| Ambiente | Sala compartilhada (coworking de saúde) ou home office; atendimento online também |
| Volume | ~25-40 pacientes ativos; sessões na maior parte dos dias da semana |
| Maturidade digital | Alta — usa Google Calendar, planilha do Google, PIX/QR, Telegram para pacientes |
| Dor central | Fazer "tudo sozinha" — agenda, cobrança, prontuário, lembrete — em ferramentas separadas, em mobilidade entre salas e casa |
| Diferenciais buscados | Mobile-first; baixo custo; setup rápido (não quer migrar dados manualmente) |

---

## Customer Profile (lado direito — quem é o cliente)

### Customer Jobs

#### Funcionais
- CJ01 — Marcar e remarcar sessões com pacientes (com flexibilidade de horário).
- CJ02 — Manter prontuário individual de cada paciente, acessível em mobile entre consultas.
- CJ03 — Cobrar pacientes (PIX, transferência, dinheiro). *(futuro)*
- CJ04 — Emitir recibo para o paciente que pede ao convênio. *(futuro)*
- CJ05 — Lembrar a si mesma e ao paciente das sessões — reduzir esquecimentos.
- CJ06 — Declarar imposto de renda (carnê-leão) com organização contábil. *(futuro)*
- CJ11 — **Deixar o próprio paciente marcar/remarcar horário** (autoagendamento), para
  ela parar de ser o "balcão" e reduzir a troca de mensagens.

#### Sociais
- CJ07 — Manter imagem profissional (atendimento pontual, recibo correto, comunicação polida).
- CJ08 — Não misturar conta pessoal de WhatsApp com conta profissional.

#### Emocionais
- CJ09 — Sentir que tem controle do próprio negócio sem sentir que "está sempre apagando incêndio".
- CJ10 — Confiança de que não está esquecendo nada importante (paciente, cobrança, prontuário).

### Pains

| ID | Dor | Severidade |
|---|---|---|
| P01 | Pacientes esquecem sessão → perda direta de R$ 150-250 por no-show | Alta |
| P02 | Esquecimento de envio de recibo após pagamento (paciente cobra dias depois) | Média |
| P03 | Prontuário em arquivos do Notion/Word — risco de perder ou misturar | Alta |
| P04 | Comunicação no WhatsApp pessoal — invasão da vida privada | Alta |
| P05 | Cobrança via PIX manual — esquece de conferir se chegou | Média |
| P06 | Não sabe quanto faturou no mês até abrir 4 ferramentas | Média |
| P07 | Sistemas existentes (Agenday, etc.) cobram por mês "caro pra quem tem 30 pacientes" | Alta |
| P08 | Mudar de ferramenta exige migrar manualmente todos os pacientes (alto custo de switching) | Média |
| P09 | Atender entre salas (mobilidade) — não funciona bem em desktop, exige solução mobile | Alta |
| P10 | Risco LGPD ela não sabe se está em conformidade ao usar Google Drive para arquivos clínicos | Alta |
| P11 | Fica o dia todo respondendo WhatsApp para marcar/remarcar horário — vira "secretária de si mesma" | Alta |

### Gains

#### Required
- G01 — Confidencialidade dos dados do paciente.
- G02 — Acesso de qualquer lugar (mobile + desktop).

#### Expected
- G03 — Lembretes automáticos.
- G04 — Cobrança e recibo em poucos cliques.
- G05 — Backup automático.

#### Desired
- G06 — Visão clara de quanto faturou no mês, sem montar planilha.
- G07 — Conformidade LGPD que ela possa apresentar se um paciente perguntar.
- G08 — Importação rápida (CSV) da base de pacientes que ela já tem.

#### Unexpected (potencialmente "wow")
- G09 — Geração automática do relatório anual para carnê-leão.
- G10 — App PWA que funciona offline em sala sem WiFi.
- G11 — Templates de mensagem profissional (não precisa pensar no texto).

---

## Value Map (lado esquerdo — o que o produto oferece)

### Products & Services

- PS01 — App web responsivo mobile-first com PWA instalável.
- PS02 — Agenda + prontuário integrados em uma conta única (sem add-ons).
- PS03 — Cobrança via PIX dinâmico (QR code) com baixa automática quando confirmado. *(futuro)*
- PS04 — Engine de lembretes via WhatsApp Business (não pessoal) e e-mail.
- PS05 — Importação de pacientes via CSV no onboarding.
- PS06 — Conformidade LGPD pronta (consentimento embutido, criptografia, export).
- PS07 — Resumo financeiro mensal automático em uma tela. *(futuro)*
- PS08 — Plano "Autônomo" com preço acessível (a definir; foco em diferenciação contra Agenday).
- PS09 — **Portal de autoagendamento do paciente:** o paciente vê os horários livres da
  Marla e marca/remarca sozinho, dentro das regras que ela definir.

### Pain Relievers

| Atende dor | Como o produto alivia |
|---|---|
| P01 (no-show) | PS04 envia lembretes 24h e 2h antes via WhatsApp, com confirmação 1 clique |
| P02 (recibo esquecido) | PS03 emite recibo automático ao registrar pagamento *(futuro)* |
| P03 (prontuário disperso) | PS02 com prontuário criptografado, *append-only* |
| P04 (WhatsApp pessoal) | PS04 usa WhatsApp Business — separação total da conta pessoal |
| P05 (cobrança manual) | PS03 com PIX dinâmico e baixa automática por webhook do banco *(futuro)* |
| P06 (sem visão financeira) | PS07 — uma tela com fatura do mês *(futuro)* |
| P07 (preço alto) | PS08 — plano específico para autônomo (escopo enxuto) |
| P08 (migração) | PS05 — importação CSV em <5 min |
| P09 (mobilidade) | PS01 — mobile-first com PWA offline |
| P10 (LGPD) | PS06 — pronto pra conformidade |
| P11 (vira secretária de si mesma) | PS09 — autoagendamento: o paciente marca sozinho nos horários livres |

### Gain Creators

| Atende ganho | Como o produto entrega |
|---|---|
| G01 (confidencialidade) | Criptografia em repouso + RBAC (Marla é única dona dos dados) |
| G02 (acesso de qualquer lugar) | PS01 — qualquer browser, qualquer device |
| G03 (lembretes) | PS04 |
| G04 (cobrança) | PS03 *(futuro)* |
| G05 (backup) | Backup automático diário |
| G06 (visão financeira) | PS07 *(futuro)* |
| G07 (LGPD) | PS06 + página exportável "como tratamos seus dados" |
| G08 (importação) | PS05 |
| G09 (carnê-leão) | Relatório anual exportável (CSV/PDF) com receita e descrição padrão *(futuro)* |
| G10 (offline) | PWA com cache local e sync ao reconectar |
| G11 (templates) | Templates pré-prontos por tipo de comunicação |
| Autonomia da agenda (CJ11) | PS09 — paciente marca/remarca sozinho dentro das regras da Marla |

---

## Resumo do encaixe: Tarefa → Dor → Ganho → Produto

Visão linha-a-linha (uma "história" de valor por tarefa), como recomendado na revisão —
cada tarefa cruza com uma dor, um ganho esperado e o produto/serviço que entrega:

| Tarefa (Job) | Dor associada | Ganho esperado | Produto/Serviço |
|---|---|---|---|
| CJ01 Marcar/remarcar | P01 no-show | G03 lembretes | PS04 lembretes |
| CJ11 Deixar paciente marcar | P11 vira secretária de si mesma | Autonomia da agenda | PS09 autoagendamento |
| CJ02 Manter prontuário | P03 prontuário disperso | G01 confidencialidade | PS02 prontuário criptografado |
| CJ05 Lembrar das sessões | P01 no-show | G03 lembretes | PS04 lembretes |
| CJ07/CJ08 Imagem profissional / separar WhatsApp | P04 WhatsApp pessoal | G07 LGPD/imagem | PS04 WhatsApp Business + PS06 |
| (mobilidade entre salas) | P09 mobilidade | G02 acesso de qualquer lugar | PS01 mobile-first |
| CJ03/CJ04 Cobrar/emitir recibo *(futuro)* | P02/P05 | G04 cobrança | PS03 *(futuro)* |
| CJ06 Carnê-leão *(futuro)* | P06 sem visão financeira | G06/G09 | PS07/relatório anual *(futuro)* |

---

## Fit (encaixe entre dores e alívios)

| Pain | Pain Reliever | Confirmação na entrevista com Marla |
|---|---|---|
| P09 (mobilidade) | PS01 (mobile-first PWA) | Marla descreveu rotina entre 3 salas diferentes na semana — "preciso abrir no celular" |
| P04 (WhatsApp pessoal) | PS04 (WhatsApp Business) | Marla mencionou ter recebido foto de paciente em horário de domingo à noite |
| P07 (preço) | PS08 (plano autônomo) | Marla disse "Agenday cobra por usuário, sou só eu, não compensa" |
| P10 (LGPD) | PS06 | Marla nunca falou explicitamente — é uma dor latente que o produto destrava |

**Encaixe forte:** mobilidade, WhatsApp profissional, cobrança PIX, recibo automático.
**Encaixe a validar:** PWA offline (G10) — depende da realidade de conectividade dela; carnê-leão (G09) — pode ser "wow" ou irrelevante se ela já usa contador.

---

## Próximos passos para validação

1. Revalidar com a Marla: o que ela define como "preço acessível" para o plano autônomo?
2. Testar o fluxo de importação CSV com a base real dela (~30 pacientes).
3. Validar se PWA offline (G10) é realmente útil ou é um "engenharia bonita sem demanda".
4. Confirmar quais campos de prontuário ela usa hoje (para garantir que o sistema cobre).

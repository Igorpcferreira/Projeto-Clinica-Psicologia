# Wireframes Mobile — Portal do Paciente e Visões Mobile

**Autor:** Pedro Lourençoni Lima
**Co-autoria:** complementa `mateus/Wireframes_ASCII.md`
**Foco:** experiência mobile-first para o Paciente (autoatendimento) e versões mobile das telas do Profissional autônomo (Marla)

> Largura-alvo: 360px (Android base) e 390px (iPhone). Toda interação deve ser confortável a uma só mão (botões primários no terço inferior).

---

## Rastreabilidade Tela ↔ Caso de Uso

Cada tela liga-se a um ou mais casos de uso (pode ser 1:N). Verificação: toda tela ativa tem
caso de uso e todo caso de uso tem tela.

| Tela | Caso(s) de uso | Observação |
|------|----------------|------------|
| M01_login | UC_Autenticar (RF03/RF04) | — |
| M02_home_portal | UC_Portal_Paciente (RF31), UC04 confirmação (RF12), UC_Cancelar_Remarcar (RF08) | — |
| M03_marcar_consulta | UC01 Agendar Consulta (RF06–RF10) | autoagendamento |
| ~~M04_recibos~~ | ~~UC_Portal (recibos)~~ | **Futuro — módulo financeiro (RF32)** |
| M05_meus_dados | UC_Portal_Paciente (RF33) | — |
| M06_privacidade | UC_Consentimento_LGPD (RF28), UC_Portabilidade (RF29), UC_Solicitar_Anonimizacao (RF30), UC_Exportar_Prontuario (RF18) | — |
| M07_agenda_diaria | UC01 (visão profissional) | — |
| M08_iniciar_sessao | UC03 Registrar Atendimento (RF15) | — |
| M09_anotacao_rapida | UC02 Manter Prontuário (RF16) | — |
| M10_encerrar_sessao | UC03 Registrar Atendimento (RF15) | a parte de pagamento da tela vira *futuro* |

> **Telas de caso de uso ainda sem protótipo mobile** (a criar / versão desktop): cadastros
> (RF01, RF02, RF05), templates (RF13), relatórios (RF23–RF25), anamnese (RF16) e auditoria
> (RF27). Listadas na [Matriz de Rastreabilidade](../../05-matriz-rastreabilidade/Matriz_Rastreabilidade.md).

---

## Tela M01 — Login Paciente (mobile)

```
┌──────────────────────┐
│                      │
│     [LOGO]           │
│                      │
│  Olá! Bem-vindo      │
│  ao seu portal.      │
│                      │
│ ┌──────────────────┐ │
│ │ E-mail           │ │
│ └──────────────────┘ │
│                      │
│ ┌──────────────────┐ │
│ │ Senha       [👁] │ │
│ └──────────────────┘ │
│                      │
│   Esqueci a senha    │
│                      │
│ ┌──────────────────┐ │
│ │     ENTRAR       │ │
│ └──────────────────┘ │
│                      │
│ ───── ou ─────       │
│                      │
│ Entrar com link      │
│ enviado por e-mail   │
│                      │
└──────────────────────┘
```

- "Magic link" como alternativa para pacientes que não querem criar senha.

---

## Tela M02 — Home do Portal (mobile)

```
┌──────────────────────┐
│ ☰ Olá, Maria      🔔│
├──────────────────────┤
│                      │
│  PRÓXIMA SESSÃO      │
│ ┌──────────────────┐ │
│ │ Quinta, 10/05    │ │
│ │ 14:00 - Karla    │ │
│ │ Online           │ │
│ │                  │ │
│ │ [Confirmar]      │ │
│ │ [Remarcar]       │ │
│ └──────────────────┘ │
│                      │
│  AÇÕES               │
│ ┌──────────────────┐ │
│ │ + Marcar consulta│ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ 📄 Recibos       │ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ 👤 Meus dados    │ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ 🔒 Privacidade   │ │
│ │   (LGPD)         │ │
│ └──────────────────┘ │
│                      │
└──────────────────────┘
```

- Tom conversacional ("Olá, Maria").
- "Confirmar" sessão como CTA primário (reduz no-show).

---

## Tela M03 — Marcar Consulta (Paciente — autoatendimento)

```
┌──────────────────────┐
│ ← Marcar consulta    │
├──────────────────────┤
│                      │
│  PASSO 1 de 4        │
│  ████░░░░░░░░░░      │
│                      │
│  Com qual            │
│  profissional?       │
│                      │
│ ┌──────────────────┐ │
│ │ 👤 Karla Niana   │ │
│ │   Psicologia     │ │
│ │   adulta         │ │
│ │   ▶              │ │
│ └──────────────────┘ │
│                      │
│ ┌──────────────────┐ │
│ │ 👤 Marla Gonç.   │ │
│ │   TCC, ansiedade │ │
│ │   ▶              │ │
│ └──────────────────┘ │
│                      │
│ ┌──────────────────┐ │
│ │ 👤 Carlos M.     │ │
│ │   Psicologia     │ │
│ │   infantil       │ │
│ │   ▶              │ │
│ └──────────────────┘ │
│                      │
└──────────────────────┘
```

```
PASSO 2: Quando?
┌──────────────────────┐
│ ← Marcar (2 de 4)    │
│  ████████░░░░░░      │
│                      │
│  Que dia funciona    │
│  pra você?           │
│                      │
│ MAIO 2026            │
│ S T Q Q S S D        │
│ . . . . 1 2 3        │
│ 4 5 6 7 8 9 10       │
│ ●1●1●1●●1●           │
│ 11 12 13 14 ...      │
│ ●  ●  .  ●           │
│                      │
│ ● = horários livres  │
│                      │
│ Selecione um dia     │
│                      │
└──────────────────────┘

PASSO 3: Horário
PASSO 4: Confirmar (modalidade, observação opcional, consentimento)
```

- **UC:** UC01 (Cenário Alternativo — Autoatendimento)
- Wizard de 4 passos para reduzir cognição.

---

## Tela M04 — Recibos (lista mobile)

```
┌──────────────────────┐
│ ← Meus recibos       │
├──────────────────────┤
│                      │
│ Período: 2026 ▾      │
│                      │
│ ┌──────────────────┐ │
│ │ Recibo #2026/123 │ │
│ │ 03/05/2026       │ │
│ │ R$ 200,00        │ │
│ │           [📥]   │ │
│ └──────────────────┘ │
│                      │
│ ┌──────────────────┐ │
│ │ Recibo #2026/098 │ │
│ │ 19/04/2026       │ │
│ │ R$ 200,00        │ │
│ │           [📥]   │ │
│ └──────────────────┘ │
│                      │
│ ...                  │
│                      │
└──────────────────────┘
```

- Botão de download isolado, alvo de toque ≥ 44px.

---

## Tela M05 — Meus Dados (Paciente — atualizar contatos)

```
┌──────────────────────┐
│ ← Meus dados         │
├──────────────────────┤
│                      │
│ Maria Luiza          │
│ ─────────────        │
│                      │
│ E-MAIL               │
│ ┌──────────────────┐ │
│ │ maria@exemplo.com│ │
│ └──────────────────┘ │
│                      │
│ TELEFONE             │
│ ┌──────────────────┐ │
│ │ +55 62 9 9999... │ │
│ └──────────────────┘ │
│                      │
│ CANAL DE LEMBRETE    │
│ (●) WhatsApp         │
│ ( ) E-mail           │
│ ( ) Ambos            │
│                      │
│ ┌──────────────────┐ │
│ │     SALVAR       │ │
│ └──────────────────┘ │
│                      │
└──────────────────────┘
```

---

## Tela M06 — Privacidade / LGPD (Paciente)

```
┌──────────────────────┐
│ ← Privacidade        │
├──────────────────────┤
│                      │
│ SEU CONSENTIMENTO    │
│                      │
│ ✓ Aceito em          │
│   10/03/2026         │
│   Versão 1.0.0       │
│                      │
│ [Ver termo completo] │
│                      │
│ ──────────────────   │
│                      │
│ SEUS DIREITOS        │
│                      │
│ ┌──────────────────┐ │
│ │ 📥 Baixar meus   │ │
│ │   dados          │ │
│ │  (Portabilidade) │ │
│ └──────────────────┘ │
│                      │
│ ┌──────────────────┐ │
│ │ 🗑 Solicitar     │ │
│ │  exclusão        │ │
│ │  (Anonimização)  │ │
│ └──────────────────┘ │
│                      │
│ ┌──────────────────┐ │
│ │ ⛔ Revogar       │ │
│ │   consentimento  │ │
│ └──────────────────┘ │
│                      │
└──────────────────────┘
```

- **UC:** UC_Portabilidade, UC_Anonimizar, UC_Consentimento_LGPD
- **Crítico:** ações destrutivas (revogar, excluir) exigem confirmação dupla + aviso de consequência (perda de acesso ao portal).

---

## Tela M07 — Agenda Diária Mobile (Profissional autônomo — Marla)

```
┌──────────────────────┐
│ < Hoje, 7 mai >    ⚙│
├──────────────────────┤
│                      │
│ 6 sessões            │
│ 78% ocupação         │
│                      │
│ ┌──────────────────┐ │
│ │ 09:00 ━━━━━━━━━━ │ │
│ │ Ana J.           │ │
│ │ Online ● Confirm.│ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ 10:00 ━━━━━━━━━━ │ │
│ │ João S. (slot    │ │
│ │ vazio? toque)    │ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ 11:00 ━━━━━━━━━━ │ │
│ │ Maria L.         │ │
│ │ Presen. ● Agendado│ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ 14:00 ━━━━━━━━━━ │ │
│ │ Carlos R.        │ │
│ └──────────────────┘ │
│                      │
│   [+ Agendar]        │
│                      │
└──────────────────────┘
```

- Single-day view por padrão; semana é gesto pinch-out.
- FAB (Floating Action Button) "+" no canto inferior direito.

---

## Tela M08 — Iniciar Sessão (Profissional, mobile)

```
┌──────────────────────┐
│ ← Sessão atual       │
├──────────────────────┤
│                      │
│ Maria Luiza          │
│ 11:00 - 11:50        │
│ Presencial           │
│                      │
│ ┌──────────────────┐ │
│ │ 🟢 Iniciar sessão│ │
│ └──────────────────┘ │
│                      │
│ Histórico recente    │
│ • 03/05 — Sessão...  │
│ • 26/04 — Sessão...  │
│ • 19/04 — Sessão...  │
│ [Ver prontuário]     │
│                      │
│ Notas rápidas        │
│ ┌──────────────────┐ │
│ │ (aparece após    │ │
│ │ iniciar)         │ │
│ └──────────────────┘ │
│                      │
└──────────────────────┘
```

- Após "Iniciar sessão", começa cronômetro e abre área de notas (que vira EvolucaoSessao ao finalizar).

---

## Tela M09 — Anotação rápida durante sessão (mobile)

```
┌──────────────────────┐
│ ⏸ 27:14              │
│ Maria L. — em sessão │
├──────────────────────┤
│                      │
│ ┌──────────────────┐ │
│ │                  │ │
│ │ Anote durante a  │ │
│ │ sessão...        │ │
│ │                  │ │
│ │                  │ │
│ │                  │ │
│ │                  │ │
│ │                  │ │
│ └──────────────────┘ │
│                      │
│ 💾 Rascunho salvo    │
│                      │
│ [Pausar] [Encerrar]  │
│                      │
└──────────────────────┘
```

- Auto-save garante que nada se perde mesmo se app fechar.
- Pausa para lidar com interrupções.

---

## Tela M10 — Encerrar Sessão (mobile, profissional autônomo)

```
┌──────────────────────┐
│ Encerrar sessão      │
├──────────────────────┤
│                      │
│ Duração: 47 min      │
│                      │
│ Compareceu?          │
│ (●) Sim   ( ) Não    │
│                      │
│ Tipo de evolução     │
│ ┌──────────────────┐ │
│ │ Sessão regular ▾ │ │
│ └──────────────────┘ │
│                      │
│ Pagamento agora?     │
│ (●) Sim, R$ 200      │
│ ( ) Não, depois      │
│                      │
│ Forma:               │
│ ( ) Dinheiro         │
│ (●) PIX              │
│ ( ) Cartão           │
│                      │
│ ┌──────────────────┐ │
│ │ Encerrar e enviar│ │
│ │ recibo           │ │
│ └──────────────────┘ │
│                      │
└──────────────────────┘
```

- Combina UC02 (publicar evolução) e UC03 (registrar atendimento + pagamento) num único fluxo otimizado para autônomo.
- Após encerrar: PIX QR aparece, paciente paga, sistema baixa automático e envia recibo.

---

## Lista consolidada de telas mobile

| # | Tela | Persona | UC |
|---|---|---|---|
| M01 | Login Paciente | Paciente | UC_Autenticar |
| M02 | Home Portal | Paciente | UC_Portal_Paciente |
| M03 | Marcar Consulta (wizard) | Paciente | UC01 (autoatendimento) |
| M04 | Lista de Recibos | Paciente | UC_Portal_Paciente |
| M05 | Meus Dados | Paciente | UC_Portal_Paciente |
| M06 | Privacidade/LGPD | Paciente | UC_Portabilidade, UC_Anonimizar |
| M07 | Agenda Diária | Profissional autônomo | UC01 |
| M08 | Iniciar Sessão | Profissional | UC03 |
| M09 | Anotação rápida | Profissional | UC02 |
| M10 | Encerrar Sessão | Profissional autônomo | UC02 + UC03 |

> Estas telas mobile complementam as 15 telas do `mateus/Wireframes_ASCII.md`. Total geral: **25 telas** para o Mateus traduzir no Figma (ou divididas: Mateus 15 + Pedro 10).

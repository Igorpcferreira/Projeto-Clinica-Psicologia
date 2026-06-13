# UC_Anamnese — Realizar Anamnese

**Autor:** Equipe (revisão da Entrega 2)
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 6 (formato *fully dressed*)

---

> **Por que um caso de uso separado?** A **anamnese** é a *primeira* entrevista clínica e
> tem natureza diferente da evolução de rotina: coleta o **histórico de vida** do paciente
> (família, desenvolvimento, queixa) e é o momento em que o **prontuário é aberto** e o
> paciente é **vinculado ao profissional**. Por isso ela não cabe dentro do
> [UC02 Manter Prontuário](UC02_Manter_Prontuario.md) — quando se registra evolução, a
> anamnese já aconteceu.

## Identificação

| Campo | Valor |
|---|---|
| **ID** | UC_Anamnese |
| **Nome** | Realizar Anamnese |
| **Escopo** | Sistema de Gestão para Clínica de Psicologia |
| **Nível** | Objetivo do usuário |
| **Ator Principal** | Profissional (psicólogo) |
| **Atores Secundários** | Sistema de Auditoria |
| **Frequência** | Baixa — uma vez por paciente, no início do acompanhamento |

## Stakeholders e Interesses

| Stakeholder | Interesse |
|---|---|
| Profissional | Coletar histórico completo para traçar o quadro do paciente |
| Paciente | Ter seu histórico registrado com sigilo |
| Clínica | Prontuário aberto corretamente, conforme CFP |

## Pré-condições

- PRE01 — Profissional autenticado.
- PRE02 — Paciente cadastrado com consentimento LGPD vigente.
- PRE03 — Paciente **ainda não possui prontuário** com este profissional.

## Pós-condições

- POS01 — Prontuário do paciente **criado** e vinculado ao profissional.
- POS02 — Registro de anamnese gravado como a primeira entrada do prontuário.
- POS03 — Log de auditoria registra a abertura do prontuário.

---

## Cenário Principal de Sucesso

1. O **Profissional** seleciona o paciente para iniciar a anamnese.
2. O **sistema** abre o prontuário do paciente e apresenta o **roteiro de anamnese**
   (histórico familiar, desenvolvimento, queixa principal, antecedentes).
3. O **Profissional** registra as informações coletadas na entrevista.
4. O **Profissional** confirma o encerramento da anamnese.
5. O **sistema** grava a anamnese, **vincula o paciente ao profissional** e registra auditoria.
6. O **sistema** confirma que o prontuário está aberto e pronto para receber evoluções
   (segue para [UC02](UC02_Manter_Prontuario.md) nas próximas sessões).

## Fluxos alternativos

### 1a. Paciente já possui prontuário com o profissional
- 1a1. O **sistema** informa que a anamnese já foi realizada e direciona para o registro de
  evolução ([UC02](UC02_Manter_Prontuario.md)).

## Requisitos especiais

- RE01 — Conteúdo da anamnese criptografado em repouso (RNF-SE-02, transversal).
- RE02 — Acesso restrito ao profissional responsável (RN01 do prontuário).

## Regras de Negócio

- **RN01** — A anamnese é realizada pelo próprio profissional que conduzirá o acompanhamento.
- **RN02** — A abertura do prontuário cria o vínculo profissional ↔ paciente.

## Rastreabilidade

- Atende RFs: RF16 (prontuário — anamnese).
- Atende necessidades: N02 (Prontuário seguro).
- Casos de uso relacionados: [UC02 Manter Prontuário](UC02_Manter_Prontuario.md).
- Telas relacionadas: `Tela_Anamnese` (a criar).
- Entidades envolvidas: Paciente, Profissional, Prontuario, EvolucaoSessao (tipo ANAMNESE), VinculoProfissionalPaciente.

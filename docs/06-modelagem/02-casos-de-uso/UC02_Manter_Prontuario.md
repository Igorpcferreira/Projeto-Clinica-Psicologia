# UC02 — Manter Prontuário Eletrônico

**Autor:** Igor Pinto de Castro Ferreira
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 6 (formato *fully dressed*)

---

## Identificação

| Campo | Valor |
|---|---|
| **ID** | UC02 |
| **Nome** | Manter Prontuário Eletrônico |
| **Escopo** | Sistema de Gestão para Clínica de Psicologia |
| **Nível** | Objetivo do usuário |
| **Ator Principal** | Profissional (psicólogo) |
| **Atores Secundários** | Sistema de Auditoria, Sistema de Armazenamento de Anexos |
| **Frequência** | Alta — uma evolução por sessão atendida |

## Stakeholders e Interesses

| Stakeholder | Interesse |
|---|---|
| Profissional | Registrar evolução clínica de forma rápida, segura e juridicamente válida |
| Paciente | Confidencialidade absoluta; possibilidade de portabilidade |
| Clínica | Conformidade com CFP nº 11/2018 e LGPD; continuidade do prontuário em caso de saída do profissional |
| Auditor / órgão regulador | Acesso controlado e log completo |

## Pré-condições

- PRE01 — Profissional autenticado com papel "Profissional" ou "Admin".
- PRE02 — Paciente cadastrado com consentimento LGPD vigente.
- PRE03 — Profissional possui vínculo ativo com o paciente (foi atribuído ou já registrou sessão prévia).

## Pós-condições

- POS01 — Prontuário criado/atualizado com evolução nova.
- POS02 — Versão anterior preservada (immutability — evoluções não são editadas, só anexadas com correção).
- POS03 — Anexos persistidos em storage criptografado, com referência no prontuário.
- POS04 — Log de auditoria registra autor, timestamp, IP e operação (create/update/read).
- POS05 — Em caso de exportação para o paciente, registrar consentimento adicional para a exportação.

---

## Cenário Principal de Sucesso — Registrar Evolução de Sessão

1. O **Profissional** acessa o registro do atendimento concluído.
2. O **sistema** apresenta o prontuário do paciente: anamnese, evoluções anteriores em ordem cronológica, anexos, e botão "Nova evolução".
3. O **Profissional** seleciona "Nova evolução".
4. O **sistema** apresenta formulário com: data/hora (preenchidas automaticamente), tipo de evolução (sessão regular, retorno, alta, encaminhamento), texto livre, anexos opcionais.
5. O **Profissional** preenche o texto da evolução. O sistema salva rascunho automaticamente a cada 30 segundos.
6. O **Profissional** opcionalmente anexa arquivos (até 10 MB cada, formatos PDF/imagem/áudio).
7. O **Profissional** confirma a publicação da evolução.
8. O **sistema** valida regras (RN01 a RN05), persiste a evolução com versionamento *append-only*, criptografa anexos e registra log de auditoria.
9. O **sistema** confirma o registro e atualiza o prontuário visível.

## Extensões

### 2a. Profissional sem vínculo com o paciente
2a1. O **sistema** bloqueia o acesso e registra tentativa no log de auditoria com severidade ALTA.
2a2. O **sistema** retorna mensagem "Acesso negado — paciente não atribuído a você".

### 5a. Conexão perdida durante edição
5a1. O **sistema** mantém rascunho local (LocalStorage) e sincroniza ao restabelecer.
5a2. Se houver conflito (outro profissional editou), o sistema bloqueia merge automático e exige resolução manual.

### 6a. Anexo excede tamanho máximo
6a1. O **sistema** rejeita o upload e sugere compressão.

### 6b. Anexo de tipo não permitido
6b1. O **sistema** rejeita e lista tipos suportados.

### 8a. Erro de criptografia / armazenamento
8a1. O **sistema** preserva o rascunho, marca como "PENDENTE" e notifica o profissional.
8a2. Operação automática de retry em background (até 3 tentativas).

### *a. Edição corretiva (não substitutiva)
\*a1. Profissional pode adicionar uma evolução do tipo "Correção" referenciando uma evolução anterior. A original permanece imutável.
\*a2. Audit log registra a correção apontando para o ID da evolução original.

---

## Cenários adicionais

### Cenário B — Visualizar histórico de evoluções

1. Profissional acessa o prontuário do paciente.
2. Sistema apresenta timeline ordenada por data decrescente.
3. Profissional pode filtrar por tipo de evolução e por intervalo de datas.
4. Cada acesso de leitura é registrado no log de auditoria.

### Cenário C — Exportar prontuário (paciente solicita portabilidade — LGPD)

1. Profissional ou Admin recebe solicitação de exportação.
2. Sistema exige reautenticação (re-prompt de senha).
3. Sistema gera PDF/A com timeline completa, hash de integridade e marca d'água com identificador único.
4. Sistema registra exportação no log com motivo informado.
5. Sistema entrega o PDF criptografado ao paciente (download via link expirável de 24h).

### Cenário D — Transferência de prontuário (saída de profissional)

1. Admin solicita transferência do conjunto de prontuários para outro profissional.
2. Sistema lista pacientes vinculados ao profissional saindo.
3. Admin atribui novo profissional para cada paciente (ou em lote).
4. Cada paciente recebe notificação informando a transferência (e direito de revogar).
5. Sistema registra a transferência no audit log.

---

## Requisitos especiais

- RE01 — Cada operação de leitura/escrita registrada no log de auditoria (RNF-SE-06).
- RE02 — Anexos criptografados em repouso com AES-256 (RNF-SE-02).
- RE03 — Salvamento automático a cada 30s ou ao mudar de campo (RNF-US-06).
- RE04 — Tempo de resposta para abrir prontuário ≤ 2s (RNF-PE-01).
- RE05 — *Append-only*: evoluções não podem ser editadas; correções são novas evoluções.
- RE06 — Acesso negado registrado com severidade ALTA e alerta ao Admin se 3 ocorrências em 1h.

## Lista de tecnologia e variações de dados

- Tipos de evolução: { SESSAO_REGULAR, RETORNO, AVALIACAO, ALTA, ENCAMINHAMENTO, CORRECAO }.
- Formatos de anexo aceitos: PDF, PNG, JPG, MP3, WAV, M4A, MP4 (limites por tipo).
- Tamanho máximo: 10 MB por arquivo, 100 MB por prontuário (sem áudios), 1 GB com áudios.
- Idioma do texto: pt-BR (padrão); suporte a copiar/colar Unicode.

## Frequência de ocorrência

- Estimativa: ~1 evolução por sessão atendida → 6-8 por profissional/dia → 100+ por dia em clínica de 15 profissionais.

## Regras de Negócio

- **RN01** — Apenas profissionais com vínculo ativo ao paciente podem registrar evolução.
- **RN02** — Evoluções são *append-only*. Correções são novas evoluções referenciando a original.
- **RN03** — Exportação para paciente exige consentimento explícito adicional (separado do consentimento de tratamento).
- **RN04** — Em caso de saída de profissional, prontuário pertence à clínica (não pode ser excluído pelo profissional).
- **RN05** — Solicitação de exclusão LGPD pelo paciente resulta em **anonimização** (não exclusão física), preservando dados clínicos por obrigação regulatória do CFP.
- **RN06** — Tempo de retenção mínima do prontuário: 20 anos após o último atendimento (Resolução CFP nº 1/2009).

## Questões em aberto

- Q01 — A clínica deseja assinatura digital nas evoluções (ICP-Brasil)?
- Q02 — Permitir gravação de áudio direto da interface (gravar e anexar)?
- Q03 — Quem pode autorizar exportação completa: somente o paciente, ou Admin pode em casos legais?
- Q04 — Compartilhamento parcial entre profissionais da mesma clínica é automático ou requer consentimento adicional?

## Rastreabilidade

- Atende RFs: RF15, RF16, RF17, RF18, RF26, RF27, RF28, RF29, RF30.
- Atende necessidades: N02 (Prontuário seguro), N05 (LGPD).
- Atende RNFs: RNF-SE-02, RNF-SE-06, RNF-SE-07, RNF-PE-01, RNF-US-06.
- Telas relacionadas: `Tela_Prontuario_Timeline`, `Tela_Nova_Evolucao`, `Tela_Anexo`, `Tela_Exportacao_LGPD`.
- Entidades envolvidas: Paciente, Profissional, Prontuario, EvolucaoSessao, Anexo, ConsentimentoLGPD, LogAuditoria.

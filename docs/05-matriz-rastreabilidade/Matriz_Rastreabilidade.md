# Matriz de Rastreabilidade

**Sistema:** Sistema de Gestão para Clínica de Psicologia
**Versão:** 1.0 · **Data:** 2026-06-13
**Parte integrante da** [ERS v2 (IEEE 830)](../04-requisitos/ERS_v2_IEEE830.md) (Anexo B) — sem documento duplicado.

---

> **Dois tipos de rastreabilidade** (conforme orientação da revisão):
> - **Vertical:** segue o requisito ao longo dos produtos de trabalho que o realizam —
>   Requisito → Caso de Uso → Classe/Método → Caso de Teste → **Tela**.
> - **Horizontal:** liga requisito a requisito (dependências entre eles).
>
> *Classe/Método* e parte dos *Casos de Teste* serão preenchidos na **Entrega 3** (projeto e
> implementação); ficam marcados como `(Entrega 3)`. As **Telas** referenciam os protótipos em
> [`docs/06-modelagem/03-prototipos/`](../06-modelagem/03-prototipos/).
>
> **Escopo:** apenas requisitos da entrega atual. Itens do módulo financeiro (RF14, RF19–RF22,
> RF32) estão em evolução futura e não aparecem aqui.

## 1. Matriz vertical (Requisito → Caso de Uso → Classe/Método → Caso de Teste → Tela)

| RF | Caso de Uso | Classe/Método | Caso de Teste | Tela / Protótipo |
|----|-------------|---------------|---------------|------------------|
| RF01 Cadastro profissional | UC_Cadastrar_Profissional | (Entrega 3) | CT-01 | Tela_Cadastro_Profissional (desktop) |
| RF02 Cadastro paciente | UC_Cadastrar_Paciente | (Entrega 3) | CT-02 | Tela_Cadastro_Paciente (desktop) |
| RF03 Autenticar | UC_Autenticar | (Entrega 3) | CT-03 | M01_login |
| RF04 Recuperar senha | UC_Autenticar (ext.) | (Entrega 3) | CT-04 | M01_login |
| RF05 Especialidades/tipos | UC_Manter_Especialidades | (Entrega 3) | CT-05 | Tela_Especialidades (desktop) |
| RF06 Agendar consulta | UC01 Agendar Consulta | (Entrega 3) | CT-06 | M03_marcar_consulta, M07_agenda_diaria |
| RF07 Evitar sobreposição | UC01 (RN01) | (Entrega 3) | CT-07 | M03_marcar_consulta |
| RF08 Cancelar/remarcar | UC_Cancelar_Remarcar | (Entrega 3) | CT-08 | M02_home_portal |
| RF09 Autoagendamento | UC01 (fluxo paciente) | (Entrega 3) | CT-09 | M03_marcar_consulta |
| RF10 Sugerir horários | UC01 (exceção *e) | (Entrega 3) | CT-10 | M03_marcar_consulta |
| RF11 Lembrete automático | UC04 Enviar Lembrete | (Entrega 3) | CT-11 | (back-end) / Tela_Templates |
| RF12 Confirmar/cancelar lembrete | UC04 (alternativos) | (Entrega 3) | CT-12 | M02_home_portal |
| RF13 Templates de mensagem | UC_Manter_Templates | (Entrega 3) | CT-13 | Tela_Templates (desktop) |
| RF15 Registrar atendimento | UC03 Registrar Atendimento | (Entrega 3) | CT-15 | M08_iniciar_sessao, M10_encerrar_sessao |
| RF16 Prontuário/anamnese | UC_Anamnese, UC02 Manter Prontuário | (Entrega 3) | CT-16 | M09_anotacao_rapida, Tela_Prontuario, Tela_Anamnese |
| RF17 Anexar arquivo | UC_Anexar_Arquivo | (Entrega 3) | CT-17 | Tela_Anexo |
| RF18 Exportar prontuário | UC_Exportar_Prontuario | (Entrega 3) | CT-18 | M06_privacidade |
| RF23 Relatório de ocupação | UC05 Gerar Relatório | (Entrega 3) | CT-23 | Tela_Relatorios |
| RF24 Relatório de no-shows | UC05 Gerar Relatório | (Entrega 3) | CT-24 | Tela_Relatorios |
| RF25 Exportar relatório PDF/CSV | UC05 Gerar Relatório | (Entrega 3) | CT-25 | Tela_Relatorios |
| RF26 RBAC | UC_Controle_Acesso | (Entrega 3) | CT-26 | (transversal) |
| RF27 Log de auditoria | UC_Consultar_Auditoria | (Entrega 3) | CT-27 | Tela_Auditoria (desktop) |
| RF28 Consentimento LGPD | UC_Consentimento_LGPD | (Entrega 3) | CT-28 | M06_privacidade |
| RF29 Portabilidade (export dados) | UC_Portabilidade_LGPD | (Entrega 3) | CT-29 | M06_privacidade |
| RF30 Anonimização | UC_Solicitar_Anonimizacao | (Entrega 3) | CT-30 | M06_privacidade |
| RF31 Ver próximas sessões | UC_Portal_Paciente | (Entrega 3) | CT-31 | M02_home_portal |
| RF33 Atualizar contato | UC_Portal_Paciente | (Entrega 3) | CT-33 | M05_meus_dados |

> **Verificação de cobertura:** todo RF em escopo tem caso de uso e tela (ou é transversal);
> toda tela ativa tem ao menos um RF/UC associado. A tela **M04_recibos** não aparece porque
> depende do módulo financeiro (futuro) — está marcada como *futuro* nos protótipos.

## 2. Matriz horizontal (Requisito ↔ Requisito)

Dependências entre requisitos (o requisito da linha **depende de / pressupõe** os das colunas):

| Requisito | Depende de |
|-----------|------------|
| RF06 Agendar | RF01 (profissional cadastrado), RF02 (paciente cadastrado), RF03 (autenticação) |
| RF07 Evitar sobreposição | RF06 |
| RF08 Cancelar/remarcar | RF06 |
| RF09 Autoagendamento | RF06, RF02 |
| RF11 Lembrete | RF06 (agendamento existente), RF13 (template) |
| RF12 Confirmar/cancelar | RF11 |
| RF15 Registrar atendimento | RF06 |
| RF16 Prontuário | RF02 (paciente), RF15 (atendimento registrado) |
| RF17 Anexar arquivo | RF16 |
| RF18 Exportar prontuário | RF16, RF28 (consentimento) |
| RF23/RF24 Relatórios | RF06, RF15 (dados de agenda e presença) |
| RF25 Exportar relatório | RF23, RF24 |
| RF29 Portabilidade | RF28 (consentimento) |
| RF30 Anonimização | RF16 |
| Todos os RFs | RF03 (autenticação), RF26 (RBAC), RF27 (auditoria) — transversais |

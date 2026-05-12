# Especificação Suplementar — Requisitos Não Funcionais (ISO/IEC 25010)

**Sistema:** Sistema de Gestão para Clínica de Psicologia
**Versão:** 1.0
**Data:** 2026-05-07
**Autor:** Igor Pinto de Castro Ferreira
**Referência:** Larman — *Utilizando UML e Padrões*, capítulo 7; ISO/IEC 25010:2011

---

## 1. Introdução

Este documento especifica os Requisitos Não Funcionais (RNFs) do sistema, classificados pelas **8 características de qualidade da ISO/IEC 25010:2011**: *Functional Suitability · Performance Efficiency · Compatibility · Usability · Reliability · Security · Maintainability · Portability*.

Cada RNF possui: ID, característica/subcaracterística, descrição, métrica/critério de aceite e prioridade (Alta, Média, Baixa).

---

## 2. Functional Suitability — Adequação Funcional

### Subcaracterísticas: Functional Completeness, Correctness, Appropriateness

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-FS-01 | 100% dos RFs de prioridade Alta devem possuir testes de aceitação automatizados. | Cobertura medida por suíte de aceitação; verificar em CI. | Alta |
| RNF-FS-02 | Funcionalidades devem produzir resultado correto conforme regras de negócio especificadas. | Zero defeitos críticos em homologação. | Alta |
| RNF-FS-03 | Funcionalidades devem ser apropriadas às tarefas dos usuários (validação por testes de usabilidade com 3+ usuários reais por papel). | NPS funcional ≥ 8 em pesquisa pós-uso. | Média |

---

## 3. Performance Efficiency — Eficiência de Desempenho

### Subcaracterísticas: Time Behaviour, Resource Utilization, Capacity

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-PE-01 | Tempo de resposta para operações de leitura (consultar agenda, abrir prontuário) deve ser ≤ 2s em 95% das requisições. | p95 ≤ 2s sob carga nominal (50 usuários simultâneos). | Alta |
| RNF-PE-02 | Tempo de resposta para operações de escrita (agendar, registrar atendimento) deve ser ≤ 3s em 95% das requisições. | p95 ≤ 3s sob carga nominal. | Alta |
| RNF-PE-03 | O sistema deve suportar pico de 200 usuários simultâneos sem degradação superior a 20% no tempo de resposta. | Teste de carga com k6 ou JMeter. | Média |
| RNF-PE-04 | Geração de relatório de agenda mensal deve concluir em ≤ 5s. | Medido em produção. | Média |
| RNF-PE-05 | Uso de memória de cada serviço backend não deve exceder 512 MB em carga nominal. | Métrica de container (Docker stats / Kubernetes metrics). | Baixa |

---

## 4. Compatibility — Compatibilidade

### Subcaracterísticas: Co-existence, Interoperability

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-CO-01 | Suporte aos navegadores Chrome, Edge, Safari e Firefox nas duas versões mais recentes. | Teste cross-browser passando. | Alta |
| RNF-CO-02 | Interface responsiva entre 320px e 1920px de largura. | Validação visual em breakpoints (320, 360, 768, 1024, 1440, 1920). | Alta |
| RNF-CO-03 | Integração com WhatsApp Business API (Meta direta, Twilio ou Z-API) sem alteração de código de domínio (apenas adaptador). | Switch de provedor por configuração. | Média |
| RNF-CO-04 | Integração com servidor SMTP por configuração. | Switch de provedor sem alteração de código. | Média |
| RNF-CO-05 | Exportação de prontuário e recibo em PDF/A para arquivamento de longo prazo. | Validação de PDF/A-3. | Baixa |

---

## 5. Usability — Usabilidade

### Subcaracterísticas: Appropriateness Recognizability, Learnability, Operability, User Error Protection, UI Aesthetics, Accessibility

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-US-01 | Onboarding de novo usuário (qualquer papel) deve ser concluído em ≤ 10 min sem treinamento prévio. | Teste com 3+ usuários por papel; tempo médio ≤ 10 min. | Alta |
| RNF-US-02 | Acessibilidade nível WCAG 2.1 AA. | Auditoria com axe-core; zero violações críticas. | Alta |
| RNF-US-03 | Mensagens de erro devem ser claras, em pt-BR, e sugerir ação corretiva. | Revisão de UX writing por 2 integrantes. | Alta |
| RNF-US-04 | Operações destrutivas (excluir paciente, cancelar sessão) exigem confirmação explícita. | Inspeção de UI. | Alta |
| RNF-US-05 | Interface mobile-first (densidade de informação otimizada para 360px primeiro). | Inspeção de protótipos Figma. | Alta |
| RNF-US-06 | Formulários longos (anamnese) devem suportar salvamento automático. | Salvar a cada 30s ou em mudança de campo. | Média |
| RNF-US-07 | Suporte a teclado completo (todas as ações acessíveis sem mouse). | Teste manual de navegação por Tab/Enter. | Média |

---

## 6. Reliability — Confiabilidade

### Subcaracterísticas: Maturity, Availability, Fault Tolerance, Recoverability

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-RE-01 | Disponibilidade do sistema ≥ 99% mensal. | Monitoramento de uptime (StatusPage ou similar). | Alta |
| RNF-RE-02 | Backups automáticos diários com retenção mínima de 30 dias. | Validação semanal de restore. | Alta |
| RNF-RE-03 | Tempo de recuperação após falha (RTO) ≤ 4h. | Testado em DR drill semestral. | Alta |
| RNF-RE-04 | Perda máxima de dados aceitável (RPO) ≤ 24h. | Validado pela política de backup. | Alta |
| RNF-RE-05 | Reenvio automático de lembretes em caso de falha de mensageria (até 3 tentativas com backoff). | Teste de injeção de falha. | Média |
| RNF-RE-06 | O sistema não deve perder dados em caso de falha durante operação de escrita (transações atômicas). | Testes de crash recovery. | Alta |

---

## 7. Security — Segurança

### Subcaracterísticas: Confidentiality, Integrity, Non-repudiation, Accountability, Authenticity

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-SE-01 | Comunicação cliente-servidor exclusivamente sobre TLS 1.2+. | Configuração de servidor; HSTS habilitado. | Alta |
| RNF-SE-02 | Dados pessoais sensíveis (CPF, prontuário, anexos) criptografados em repouso (AES-256). | Inspeção de configuração de banco e storage. | Alta |
| RNF-SE-03 | Senhas armazenadas com hashing forte (bcrypt/argon2id, custo ≥ 12). | Inspeção de código. | Alta |
| RNF-SE-04 | Autenticação multifator (MFA) opcional para Admin e Profissional. | Configurável no perfil. | Média |
| RNF-SE-05 | RBAC com papéis Admin, Profissional, Secretária, Paciente. | Inspeção de matriz de permissões. | Alta |
| RNF-SE-06 | Log de auditoria registra usuário, timestamp, IP, ação e dados antes/depois para operações sensíveis. | Inspeção de log e retenção mínima de 12 meses. | Alta |
| RNF-SE-07 | Conformidade LGPD: consentimento explícito, portabilidade, exclusão (anonimização para prontuário). | Auditoria contra checklist LGPD ANPD. | Alta |
| RNF-SE-08 | Rate limiting em autenticação (≤ 10 tentativas/min/IP). | Configuração de gateway. | Média |
| RNF-SE-09 | Sanitização de input para prevenir SQL Injection e XSS. | Code review obrigatório + scanner SAST. | Alta |
| RNF-SE-10 | Sessão com expiração após 30 min de inatividade. | Configuração. | Média |

---

## 8. Maintainability — Manutenibilidade

### Subcaracterísticas: Modularity, Reusability, Analysability, Modifiability, Testability

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-MA-01 | Cobertura de testes unitários ≥ 70% no backend. | Relatório de cobertura no CI. | Alta |
| RNF-MA-02 | Código segue guia de estilo (linter sem violações em CI). | Pipeline com ESLint/Pylint/etc. obrigatório. | Alta |
| RNF-MA-03 | Cada feature em módulo com fronteiras claras (alta coesão, baixo acoplamento). | Análise de dependências por revisor. | Média |
| RNF-MA-04 | Documentação de API publicada (OpenAPI 3 / Swagger UI). | Endpoint `/docs` acessível em ambiente de desenvolvimento. | Média |
| RNF-MA-05 | Migrações de banco versionadas e reversíveis (Flyway/Liquibase ou equivalente). | Inspeção de pasta de migrations. | Alta |
| RNF-MA-06 | Logs estruturados em JSON com correlation ID. | Inspeção de output. | Média |

---

## 9. Portability — Portabilidade

### Subcaracterísticas: Adaptability, Installability, Replaceability

| ID | Descrição | Métrica / Critério de aceite | Prioridade |
|---|---|---|---|
| RNF-PO-01 | Aplicação containerizada (Docker) com `docker-compose` para ambiente local. | Verificar `Dockerfile` e `docker-compose.yml`. | Alta |
| RNF-PO-02 | Deployment automatizado em pelo menos um provedor cloud (AWS, GCP, Azure ou Oracle). | Pipeline CD funcional. | Média |
| RNF-PO-03 | Configurações sensíveis externalizadas (variáveis de ambiente / secrets manager). | Inspeção: nenhum segredo hardcoded. | Alta |
| RNF-PO-04 | Banco de dados intercambiável entre PostgreSQL e MySQL via ORM (sem SQL específico de fornecedor). | Pipeline com testes nos dois bancos. | Baixa |

---

## 10. Resumo executivo (alvos numéricos)

| Indicador | Alvo |
|---|---|
| Disponibilidade mensal | ≥ 99% |
| p95 leitura | ≤ 2s |
| p95 escrita | ≤ 3s |
| Cobertura de testes backend | ≥ 70% |
| Onboarding novo usuário | ≤ 10 min |
| RTO | ≤ 4h |
| RPO | ≤ 24h |
| Acessibilidade | WCAG 2.1 AA |
| Criptografia em trânsito | TLS 1.2+ |
| Criptografia em repouso | AES-256 |
| Hashing de senha | bcrypt/argon2id (custo ≥ 12) |
| Pico de usuários simultâneos | 200 |

---

## 11. Rastreabilidade RNF ↔ Necessidade

| RNF | Necessidade atendida |
|---|---|
| RNF-PE-01 a 05 | (qualidade transversal) |
| RNF-CO-02 | N06 — Mobile |
| RNF-RE-01 a 06 | (qualidade transversal — viabiliza N01 e N02) |
| RNF-SE-01 a 10 | N02, N05 — LGPD e prontuário seguro |
| RNF-US-01 a 07 | (qualidade transversal — viabiliza adoção) |

# Dicionário de Dados — Sistema de Gestão para Clínica de Psicologia

**Responsável:** João Pedro Gonçalves Ferreira
**Referência:** Larman, cap. 7 · Diagramas em `docs-2/modelagem/` (`MER_Conceitual.puml`, `MER.dbml`, `MER.puml`)

---

> Este dicionário descreve as **entidades conceituais** do domínio e seus atributos
> (nome, tipo lógico, descrição). É o complemento textual do **modelo conceitual**
> (`MER_Conceitual.puml`). As chaves primárias/estrangeiras pertencem ao **modelo relacional**
> (`MER.dbml`) — etapa seguinte — e por isso não são detalhadas aqui.
>
> **Escopo desta entrega:** agendamento, atendimento, prontuário e comunicação. As entidades
> financeiras (Pagamento, Recibo) ficam para a evolução futura.

## Identidade e acesso

### Usuario
| Atributo | Tipo | Descrição |
|---|---|---|
| email | texto | Identificação de login; único |
| nome | texto | Nome do usuário |
| ativo | booleano | Indica se o acesso está habilitado |
| mfa_habilitado | booleano | Se usa segundo fator de autenticação |

### Papel
| Atributo | Tipo | Descrição |
|---|---|---|
| nome | texto | ADMIN, PROFISSIONAL, SECRETARIA ou PACIENTE |

## Domínio clínico

### Clinica
| Atributo | Tipo | Descrição |
|---|---|---|
| razao_social | texto | Nome da clínica |
| cnpj | texto | CNPJ (opcional p/ autônomo) |
| endereco | texto | Endereço |
| telefone | texto | Telefone de contato |

### Profissional
| Atributo | Tipo | Descrição |
|---|---|---|
| crp | texto | Registro no Conselho Regional de Psicologia |
| bio | texto | Mini currículo |
| ativo | booleano | Se está atendendo |
| permite_autoagendamento | booleano | Se libera o paciente a marcar sozinho |

### Especialidade
| Atributo | Tipo | Descrição |
|---|---|---|
| nome | texto | Ex.: Psicologia clínica, Fonoaudiologia |

### AgendaConfig
| Atributo | Tipo | Descrição |
|---|---|---|
| dia_semana | inteiro | 0=Domingo .. 6=Sábado |
| hora_inicio | hora | Início do expediente no dia |
| hora_fim | hora | Fim do expediente no dia |
| duracao_slot_min | inteiro | Duração padrão da sessão em minutos |

### Sala
| Atributo | Tipo | Descrição |
|---|---|---|
| nome | texto | Identificação da sala |
| tipo | texto | INDIVIDUAL ou MULTIDISCIPLINAR |
| capacidade | inteiro | Nº de pessoas suportadas |

### Paciente
| Atributo | Tipo | Descrição |
|---|---|---|
| nome | texto | Nome do paciente |
| cpf | texto | CPF (único) |
| data_nascimento | data | Data de nascimento |
| email | texto | E-mail de contato |
| telefone | texto | Telefone/WhatsApp |
| canal_preferido_lembrete | texto | WHATSAPP, EMAIL ou AMBOS |

## Agendamento e atendimento

### Agendamento
| Atributo | Tipo | Descrição |
|---|---|---|
| inicio | data/hora | Início da sessão |
| fim | data/hora | Fim da sessão |
| modalidade | texto | PRESENCIAL ou ONLINE |
| tipo_sessao | texto | ANAMNESE, RETORNO, AVALIACAO, GRUPAL |
| status | texto | AGENDADO, CONFIRMADO, REALIZADO, CANCELADO, FALTOU |
| recorrencia | texto | Regra de repetição (ex.: semanal até data); vazio se avulso |

### Atendimento
| Atributo | Tipo | Descrição |
|---|---|---|
| iniciado_em | data/hora | Início real do atendimento |
| finalizado_em | data/hora | Fim real do atendimento |
| duracao_min | inteiro | Duração em minutos |
| presenca | texto | PRESENTE ou FALTOU |
| observacoes | texto | Observações do atendimento |

## Prontuário

### Prontuario
| Atributo | Tipo | Descrição |
|---|---|---|
| aberto_em | data/hora | Abertura (na anamnese) |
| encerrado_em | data/hora | Encerramento (alta), se houver |

### EvolucaoSessao
| Atributo | Tipo | Descrição |
|---|---|---|
| tipo | texto | ANAMNESE, SESSAO_REGULAR, RETORNO, AVALIACAO, ALTA, ENCAMINHAMENTO, CORRECAO |
| texto | texto | Conteúdo clínico (criptografado em repouso) |
| registrado_em | data/hora | Data/hora do registro |

### Anexo
| Atributo | Tipo | Descrição |
|---|---|---|
| nome_arquivo | texto | Nome do arquivo anexado |
| tipo | texto | Tipo MIME (PDF, imagem, áudio) |
| tamanho | inteiro | Tamanho em bytes (limite 10 MB) |

## Comunicação

### Lembrete
| Atributo | Tipo | Descrição |
|---|---|---|
| canal | texto | WHATSAPP ou EMAIL |
| agendar_para | data/hora | Quando deve ser enviado |
| status | texto | PENDENTE, ENVIADO, CONFIRMADO, FALHADO, REENVIO |

### TemplateMensagem
| Atributo | Tipo | Descrição |
|---|---|---|
| tipo | texto | LEMBRETE_24H, LEMBRETE_2H, CONFIRMACAO |
| canal | texto | WHATSAPP ou EMAIL |
| conteudo | texto | Texto com variáveis (ex.: `{{paciente}}`) |

## LGPD e auditoria

### ConsentimentoLGPD
| Atributo | Tipo | Descrição |
|---|---|---|
| versao_termo | texto | Versão do termo aceito |
| aceito_em | data/hora | Quando foi aceito |
| revogado_em | data/hora | Quando foi revogado (se houver) |

### LogAuditoria
| Atributo | Tipo | Descrição |
|---|---|---|
| acao | texto | CREATE, UPDATE, DELETE, READ, EXPORT, LOGIN... |
| entidade | texto | Entidade afetada |
| registrado_em | data/hora | Momento do evento |
| severidade | texto | INFO, WARN, ALTA |

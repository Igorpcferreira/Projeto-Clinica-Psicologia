# Especificação de Arquitetura (Entrega 3)

**Padrão:** ISO/IEC/IEEE 42010 (simplificação) + modelo **C4** + Larman (cap. 11).

## Documentos
- [`Especificacao_Arquitetura.md`](Especificacao_Arquitetura.md) — documento principal (seções 1–8).
- [`Esquema_Operacional_BD.md`](Esquema_Operacional_BD.md) — tabelas, tipos e tamanhos (C4).
- [`Contrato_Operacao.md`](Contrato_Operacao.md) — contrato de operação (Larman cap. 11).

## Diagramas (PlantUML — gerar PNG/SVG com PlantUML)
| Nível | Arquivo | Conteúdo |
|-------|---------|----------|
| C1 | `diagramas/c1-implantacao.puml` | Diagrama de implantação / contexto externo |
| BPMN | `diagramas/bpmn-agendamento.puml` | Processo: agendar consulta |
| BPMN | `diagramas/bpmn-atendimento.puml` | Processo: registrar atendimento/evolução |
| BPMN | `diagramas/bpmn-lembrete.puml` | Processo: enviar lembrete |
| C2 | `diagramas/c2-pacotes.puml` | Diagrama de pacotes |
| C3 | `diagramas/c3-componentes.puml` | Componentes por pacote |
| C3 | `diagramas/c3-sequencia-componentes.puml` | Sequência entre componentes |
| C4 | `diagramas/c4-classes-agendamento.puml` | Classes do componente Agendamento |
| C4 | `diagramas/c4-classes-prontuario.puml` | Classes do componente Prontuário |
| C4 | `diagramas/c4-seq-criarAgendamento.puml` | Sequência da operação criarAgendamento |
| C4 | `diagramas/c4-seq-registrarEvolucao.puml` | Sequência da operação registrarEvolucao |

> Para gerar as imagens: `java -jar plantuml.jar docs/07-arquitetura/diagramas/*.puml`
> (ou colar em https://www.plantuml.com/plantuml/uml/).

## Ordem sugerida da documentação IMPRESSA (entrega final)
1. Capa: *Entrega final do Projeto de Arquitetura e Desenho de Software*
2. **Especificação de Requisitos (ERS — IEEE 830)** com, nos espaços pertinentes:
   - Lista de requisitos funcionais e não funcionais
   - Diagrama de casos de uso
   - Todos os casos de uso no formato completo
   - Protótipo de todas as telas (com explicação e conexão ao caso de uso)
   - Modelo Entidade-Relacionamento (MER)
   - Matriz de Rastreabilidade **Horizontal**
3. **Especificação de Arquitetura (42010)** — este documento (C1→C4)
4. **Anexos:** entrevistas, questionários, síntese do sistema análogo, Dicionário de Dados do MER, e Matriz de Rastreabilidade **Vertical**

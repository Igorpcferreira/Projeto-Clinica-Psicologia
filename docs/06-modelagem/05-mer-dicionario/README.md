# MER e Dicionario de Dados

**Responsavel:** Joao Pedro
**Referencia:** Larman cap. 7
**Status:** Atualizado na revisao da Entrega 2

## Arquivos
- **Modelo conceitual** (sem chaves, relacionamentos nomeados):
  [`docs-2/modelagem/MER_Conceitual.puml`](../../../docs-2/modelagem/MER_Conceitual.puml)
- **Modelo relacional** (com PK/FK — proxima etapa):
  [`docs-2/modelagem/MER.dbml`](../../../docs-2/modelagem/MER.dbml) e
  [`docs-2/modelagem/MER.puml`](../../../docs-2/modelagem/MER.puml)
- **Dicionario de dados:** [`Dicionario_Dados.md`](Dicionario_Dados.md)

## Correcoes aplicadas (feedback da professora)
- Separado o **modelo conceitual** (sem chave primaria/estrangeira, relacionamentos
  **nomeados** com cardinalidade) do **modelo relacional** (com PK/FK), que e a etapa seguinte.
- Garantido o relacionamento **EvolucaoSessao -- Profissional** ("Profissional registra Evolucao").
- Adicionada a entidade **Sala** (alocacao de sala e ponto critico do agendamento) e o
  vinculo Sala -- Agendamento.
- Removidas **Pagamento/Recibo** do escopo (modulo financeiro = evolucao futura).
- Dicionario reescrito alinhado as **entidades conceituais**.

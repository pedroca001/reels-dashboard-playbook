# Modelo de dados e cálculos

O [snapshot fictício](../exemplos/snapshot.json) é um contrato didático simplificado. O frontend e o importador novos devem implementar e versionar seu contrato; ele não é um arquivo substituível diretamente no dashboard original.

## Entidades mínimas

| Entidade | Chave | Campos essenciais |
|---|---|---|
| Página | page_id | nome, rotação, motivo, nicho confirmado opcional |
| Publicação | page_id + content_id | permalink, published_at, vínculo de arquivo opcional |
| Exportação | SHA256 | requested_at, received_at, fuso, caminhos privados de evidência |
| Observação | digest + page_id + content_id | observed_at, source, valores por métrica |
| Agendamento opcional | identidade operacional | scheduled_at, estado registrado, evidência |

IDs são strings, não números JavaScript. Datas de armazenamento têm offset ou UTC. O fuso do CSV vem da interface observada, nunca da localização presumida do operador. Na referência, datas `MM/DD/YYYY HH:MM` foram interpretadas em `America/Los_Angeles` e exibidas em Brasília. Use uma base IANA para respeitar horário de verão, sem offset fixo.

## Valores exatos e ausências

- `0` observado é zero. Campo vazio, traço, indisponível ou valor abreviado não é zero.
- Preserve a última leitura conhecida por campo com seu horário original. Views antigas e comentários recentes podem coexistir.
- Não atribua horário novo a um valor histórico reutilizado. Mostre cobertura nova e cobertura histórica separadamente.
- Nunca some todas as observações do mesmo vídeo. Para totais, use uma última leitura por métrica/publicação.

## Conciliação com um distribuidor

Primeiro, use página e permalink público verificado. Se não houver correspondência, uma regra opcional do modelo original exige candidato único pela mesma página, horário publicado entre o agendado e até dez minutos depois, legenda normalizada igual e duração com diferença máxima de um segundo. Esses limites são específicos da implementação e devem ser testados.

ID de prévia de agendamento pode diferir do ID público. Mais de um candidato continua pendente. Publicação nativa sem arquivo identificado pode aparecer por chave sintética `native:PAGE:CID`, marcada como arquivo a vincular. Não inventar SHA de arquivo nem reescrever o ledger para fechar uma pendência.

## Indicadores

```text
views conhecidas = soma da última leitura de views de cada publicado medido
comentários conhecidos = soma da última leitura de comentários de cada publicado medido
mediana = mediana das views dos publicados com leitura
comentários por mil = 1000 × soma(comentários) / soma(views)
concentração = views do maior Reel / views conhecidas da página
```

A taxa usa o mesmo conjunto com ambas as métricas. Denominador zero produz taxa indisponível. Somar alcance de vídeos não fornece pessoas únicas; views também não são pessoas únicas.

Para crescimento entre duas rodadas globais, selecione apenas vídeos publicados com views exatas nos dois exports. Some `atual - anterior`. Vídeos novos e ausentes ficam fora desse delta. Preserve correções negativas. Não misture intervalos individuais diferentes sob o rótulo entre coletas. A classificação na/fora da rotação é a atual e não prova efeito causal da retirada.

## Estados e cobertura

Diferencie publicado observado, futuro, vencido sem correspondência, processing e falha nativa comprovada. Uma categoria sem registros vale zero no resumo. Ausência do export não comprova falha. O horário da projeção não substitui o horário da coleta.

Como exemplo fictício, um inventário de 12 páginas pode coexistir com escopo de coleta de 10 quando duas não têm histórico. Explique a diferença na interface. Nichos e agenda continuam independentes das contagens.

# Construção do dashboard

## 1. Defina as fontes

Use um catálogo de páginas com ID estável, nome, situação de rotação e motivo. Mantenha um inventário completo e um subconjunto de páginas com histórico para métricas. Estar fora da rotação não remove resultados antigos.

Se existir um distribuidor de mídia, leia seu ledger via SQLite `mode=ro`, com backup consistente. Separe seu banco de métricas do banco operacional. Se não houver distribuidor, comece com catálogo e publicações observadas no export; não fabrique intenções, arquivos ou agendamentos para preencher o modelo.

## 2. Estrutura sugerida para o aplicativo a implementar

```text
app/
  AGENTS.md
  metrics.py
  native_export_adapter.mjs
  page_niches.json
  tests/
  public/
    index.html
    dashboard.js
    dashboard.css
    data.json
  netlify.toml
```

Fora do repositório, configure um runtime próprio com `history.sqlite3` e `reports/<rodada>/`. Ali ficam manifestos, CSVs brutos, backups, hashes, horários, evidências e resultados de validação. Não copiar um runtime pessoal para outro computador como regra de instalação.

`metrics.py` e os demais nomes são uma proposta de responsabilidades. Estes executáveis não vêm neste manual. O sistema original usa um coletor com esses nomes e depende de um distribuidor irmão; copiá-lo isoladamente não cria uma instalação autônoma.

## 3. Implemente as camadas

- **Preparação:** snapshot somente leitura, backup consistente e manifesto de todas as páginas com histórico.
- **Adapter de interface:** recebe a aba atual e dependências por injeção, seleciona páginas, configura período, gera e baixa relatórios pela UI documentada do agente.
- **Importador:** valida o CSV inteiro antes de persistir. Converte datas, preserva métricas ausentes e cria observações idempotentes.
- **Histórico:** mantém exports e observações imutáveis, identificados por SHA256 e página/conteúdo.
- **Projeção:** concilia conteúdo com publicações e produz somente campos permitidos para o site.
- **Frontend:** lê JSON, aplica filtros e calcula indicadores sem comandos de postagem.

Evite dependência direta do frontend em bancos de trabalho ou sessão Meta. Uma lista explícita de campos permitidos é mais segura do que remover campos conhecidos de um objeto operacional grande.

## 4. Construa as telas

Comece em modo claro, com fundo claro, tipografia legível e estados por texto mais cor. No topo, mostre última coleta e cobertura. Depois, quatro indicadores: publicados, views conhecidas, comentários e taxa conjunta. Cada total precisa informar quantos vídeos têm a métrica.

A seção de rotação mostra na/fora/não conferida, participação nas views e agenda cadastrada. O mapa por página mostra mediana por Reel, ganho comparável, maior Reel/total, cobertura e futuros. Permita ordenar sem esconder páginas arquivadas.

Nos insights, mostre amostra e intervalo. Para evitar destaque baseado em amostra mínima, o modelo de referência exige pelo menos cinco Reels medidos para concentração e pelo menos cinco Reels com ambas as métricas e cinco comentários para destacar taxa. São escolhas do produto, não requisitos Meta.

O detalhe de um Reel deve mostrar página, ID/permalink, data real, fonte, últimas leituras por campo, histórico e pendências. Favoritos, notas e filtros podem ficar no navegador, com opção de limpar. Paginação evita renderizar milhares de linhas ao mesmo tempo.

## 5. Nichos planejados

Mantenha a classificação por ID em cadastro próprio. O nicho planejado não muda Meta, rotação, acesso ou funil. Novas páginas começam sem classificação até confirmação. Rascunhos no navegador precisam de export/import e regra explícita para uma confirmação publicada substituir rascunho antigo. Views históricas não representam automaticamente resultado do nicho planejado.

## 6. Verifique antes de publicar

Use dados fictícios para desenvolver e cubra os casos de zero, ausente, repetição do mesmo CSV, páginas fora da rotação, publicação manual sem arquivo, dois relatórios e export parcial. Valide que os filtros recalculam todos os cards/insights e que a versão hospedada bloqueia métodos de alteração antes de qualquer chamada de rede.

Confira desktop e tela estreita, tabela/cards/mapa, CSV e detalhe. Se reutilizar uma interface canônica, gere a cópia hospedada com hashes de proveniência e evite editá-la separadamente.

## 7. Hospede

Para arquivos finais commitados, a configuração mínima da Netlify no aplicativo é:

```toml
[build]
  publish = "public"
```

Não colocar banco, CSV bruto, credenciais ou sessão dentro de `public`. Um repo privado não torna os arquivos do site privados: qualquer proteção do site deve ser decidida separadamente pelo proprietário. Após o deploy, abra a URL, confira totais/data e compare SHA256 dos arquivos servidos com os commitados. Não anunciar publicação apenas porque o push funcionou.

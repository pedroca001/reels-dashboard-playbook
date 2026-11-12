# Rotina de atualização

## 1. Prepare uma rodada

Releia o contrato do projeto e o estado atual. Abra o ledger operacional somente para leitura, gere backup consistente e um manifesto completo das páginas com histórico, inclusive arquivadas. Registre IDs/nomes, primeira data acompanhada e escopo. Não use uma lista fixa da coleta anterior.

Em um aplicativo novo, implemente um comando de preparação equivalente. No sistema original já existente, os comandos são:

```bash
python3 metrics.py prepare
```

O comando original requer o distribuidor configurado, cria uma pasta de rodada fora do repo e retorna seu caminho. Não execute esse comando neste manual: aqui não existe `metrics.py`.

## 2. Exporte na Meta

Use o navegador autenticado do proprietário e a interface documentada do agente. Mantenha uma aba produtiva e identidade de página conferida. Não extrair cookies, sessões ou chamar endpoints internos. Preserve o Chrome; não encerrar/reiniciar para recuperar controle.

Abra Business Suite > Conteúdo > Publicados > Exportar dados. Para cada lote:

1. Selecione as páginas do subconjunto pelo nome e confirme seleção persistida; o CSV validará o ID depois.
2. Feche o seletor e confira a quantidade final na interface. Se o último clique ainda não persistiu, reconcilie a página antes de marcar de novo.
3. Configure **Total**, **Post** e **Data de criação**. Total fornece contagens acumuladas, enquanto Diário exige outro modelo e não pode ser somado como se fosse Total.
4. Escolha período do primeiro envio acompanhado até a data atual. Este ano só cobre todo o histórico se o primeiro envio também pertence ao ano atual; na virada de ano, use intervalo personalizado.
5. Registre período, fuso observado, configuração, seleção e horário de solicitação antes de Gerar.
6. Aguarde concluída/100%. Não gerar outro pedido apenas porque demorou.
7. Registre intenção e baixe uma vez. Se houver diálogo nativo Salvar, conclua-o antes de esperar outro arquivo.

As sessões de referência observaram limite de 50 páginas por relatório. Divida um manifesto maior em conjuntos disjuntos de até 50, revalidando o limite visível. O exemplo de 60 usa 50+10. Termine e baixe um lote antes do próximo. Cada lote guarda seu manifesto, pedido, configuração, arquivo e horário.

Se a UI agrupar relatórios prontos em Baixar exportações, o menu Exportações recentes permite escolher o lote exato. Confira nome, período, quantidade e conclusão antes de clicar. Não baixar tudo de novo sem necessidade.

## 3. Valide e una os lotes

Confira existência, tamanho, hash e horário do arquivo após a intenção de download. Um evento do navegador que expira não significa que o download falhou; confira o diálogo Salvar e o filesystem antes de clicar outra vez.

Cada lote precisa ter somente IDs do seu manifesto e cobertura de páginas explicável. Página sem conteúdo pode exigir confirmação direta e registro de cobertura vazia; nunca preencher linhas por exclusão. IDs inválidos/desconhecidos, nome não vazio divergente e duplicatas bloqueiam a importação.

Nome vazio só pode usar o catálogo quando o ID é conhecido, com origem explicitamente marcada. Linha sem ID de página não ganha identidade pela ordem, pelo número de linhas ou por ser a única página ausente. Preserve o arquivo rejeitado e, após reconciliar o pedido anterior, faça no máximo uma nova tentativa consolidada para aquele problema. Se persistir, informe cobertura parcial ou bloqueio concreto.

Na união, alinhe cabeçalhos por nome. Mudanças de ordem e coluna extra opcional podem ser preservadas com união de colunas e vazios onde ausentes; campos obrigatórios devem existir em todos os lotes. Exija disjunção dos IDs de página entre lotes, unicidade de página/conteúdo e união do escopo planejado. Não deduplicar silenciosamente. Preserve os brutos e registre `combined_provenance.json` com hashes, solicitações e recebimentos por lote.

## 4. Importe e projete

Valide o CSV completo antes de persistir. Exclua fotos do conjunto de Reels, converta datas com o fuso observado e omita métricas não exatas. Use hash do export e par página/conteúdo para idempotência. O mesmo CSV não representa uma nova coleta com outro horário.

Guarde observações em histórico append-only separado. Na rodada consolidada, o horário global é o recebimento final, com os horários individuais dos lotes preservados. Não afirmar simultaneidade.

A projeção concilia publicações e metadados sem alterar o distribuidor. Falhas observadas precisam de evidência própria e associação ao mesmo agendamento; uma falha antiga não cancela uma reprogramação posterior. Valores antigos mantêm suas datas.

No sistema original, os comandos abaixo já existem; em um projeto novo, implemente os contratos equivalentes antes de usá-los:

```bash
python3 metrics.py import --run <RODADA> --csv <CSV_VALIDADO> --observed-at <ISO_COM_FUSO> --export-timezone <FUSO_IANA_OBSERVADO>
python3 metrics.py verify-ledger --run <RODADA>
python3 metrics.py build
python3 -B -m unittest discover -s tests -q
node --test tests/*.test.cjs
```

`build` atualiza o snapshot e sincroniza a interface canônica; sozinho não coleta métricas. O verificador deve comparar tabelas com o backup, além de integrity_check e foreign_key_check. Nenhum receipt, estado operacional, agenda, arquivo ou permissão deve mudar como efeito da coleta.

## 5. Trate atraso e cobertura

Compare a cobertura com as intenções, horários vencidos e coleta anterior. A Meta já omitiu publicações do dia corrente mesmo mostrando o período até hoje. Uma amostra na UI confirmou vídeos públicos ausentes do CSV; em rodada posterior eles apareceram. Isso é evidência de cobertura parcial naquelas rodadas, não garantia de atraso fixo nem motivo para inventar números.

Guarde o relatório parcial, preserve histórico e informe quantos registros ficaram sem correspondência. Se conferir uma amostra, não estenda sua situação a todas as páginas. O nome do arquivo pode terminar no dia anterior: registre a divergência e confira período observado e linhas reais.

## 6. Publique e confira

Revise o diff e versione só fonte, documentação e snapshot permitido. Não enviar CSV bruto, SQLite, screenshot privado, sessão ou configuração local. Se existirem mudanças concorrentes do usuário, inclua somente os arquivos da tarefa.

Faça commit/push para o repo do aplicativo conectado à hospedagem. Aguarde o deploy e compare os hashes SHA256 dos arquivos servidos com os locais, especialmente `data.json` e interface/proveniência. Abra o site e confira horário, totais, cobertura e rotação; identifique filtros salvos antes de comparar números.

Depois de observar o resultado, guarde evidência, registre versão/commit e finalize com URL, números e limitações relevantes. A coleta é sob demanda por padrão. Agendamento recorrente só deve ser criado por pedido explícito e com dependências reais documentadas.

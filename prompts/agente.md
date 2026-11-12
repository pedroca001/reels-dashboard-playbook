# Prompt para construir

Copie o bloco abaixo para o agente no workspace escolhido.

```text
Construa um dashboard independente de métricas de Reels usando este playbook.
Leia README.md e os documentos de docs/ antes de implementar.

Este repositório é um manual: você deve criar o aplicativo, o coletor e os testes no destino apropriado. Não suponha que metrics.py ou uma instalação privada estejam disponíveis.

Descubra primeiro o catálogo autorizado, a fonte de publicações/agendamentos, o ambiente local e a conta de hospedagem. Se não houver distribuidor, use catálogo mais export nativo sem inventar vínculos operacionais. Faça progresso com fixtures fictícias enquanto resolve acessos necessários.

Implemente preparação com manifesto/backup, export nativo Meta em lotes de até o limite observado, validação completa, histórico append-only, conciliação sem ambiguidade, snapshot por allowlist e frontend estático. Mantenha a coleta separada de postagens, agenda e permissões.

Use IDs como strings e datas com fuso. Zero observado é zero; ausente continua ausente ou preserva a última leitura com sua data original. Totais usam uma leitura por campo. Ganho entre coletas usa apenas os mesmos vídeos com views exatas em ambos os exports globais, preservando correções negativas e excluindo novos/ausentes.

Crie resultados, cobertura, filtros, mapa da rotação, tabela/cards, comparação por página/áudio, detalhe/histórico, CSV e nichos planejados com confirmação humana. Não altere a rotação por ranking. Deixe explícito o que é rascunho local e o que foi publicado para todos os navegadores.

Use somente o controle de navegador documentado e preserve o Chrome. Não copie sessões, bancos ou dados pessoais deste manual ou de outro projeto. Não contorne bloqueios. Reconcile ações incertas antes de repetir.

Teste os casos críticos descritos em docs/diagnostico.md. Faça uma coleta real autorizada quando houver acesso e marque qualquer lacuna. Publique no repo/hospedagem autorizados e confira URL, horário, totais e hashes. Informe o resultado e limitações reais. Não criar cron ou heartbeat sem pedido.
```

# Prompt para atualizar uma instalação pronta

```text
Atualize o dashboard desta instalação. Leia o contrato e identifique o último snapshot e a fonte atual.
Prepare manifesto completo e backup; inclua páginas com histórico fora da rotação. Exporte Total/Post/Data de criação com período completo e fuso observado, em lotes disjuntos conforme limite da UI. Gere e baixe cada lote uma vez, preservando intenção, evidência, hashes e horários.
Valide identidade, cobertura, campos e duplicatas antes de importar; consolide por nome de coluna. Preserve histórico e valores ausentes, concilie somente com identidade única. Nunca mudar postagens, agenda, acesso, rotação ou nichos por causa da atualização.
Verifique o banco operacional, construa snapshot permitido, rode testes pertinentes e revise o diff. Publique apenas artefatos seguros. Confira deploy por hash e no navegador, observando filtros locais. Finalize com link, totais e lacunas da Meta. Não inventar métricas nem repetir envios ausentes.
```

# Diagnóstico e manutenção

| Sintoma | Conferir | Ação |
|---|---|---|
| Exportador não permite Gerar | seleção persistida e limite visível | dividir manifesto, conferir contagem final |
| Nenhum CSV aparece | diálogo nativo Salvar, arquivo novo e temporário | concluir diálogo; reconciliar antes de repetir |
| CSV veio sem identidade | ID da página/conteúdo e manifesto | rejeitar antes de persistir; não atribuir por exclusão |
| Colunas mudaram | nomes obrigatórios e extras opcionais | alinhar por nome; preservar dados brutos |
| Publicações recentes não vieram | período/fuso, horários e amostra UI | manter pendente e explicar cobertura parcial |
| Views ficaram vazias | leitura exata anterior daquele campo | manter data antiga; nunca converter em zero |
| ID da prévia difere do publicado | permalink público e metadados únicos | conciliar só sem ambiguidade |
| Site mostra total menor | filtros locais, rotação e período | conferir o recorte antes de concluir perda de dados |
| Push passou, site continua antigo | deploy, branch, pasta e hash servido | aguardar/reconciliar deploy; não alegar publicação |
| Build sem dados novos | observed_at versus generated_at | build é projeção, precisa de export novo |
| Nicho editado em um navegador não aparece em outro | rascunho local e base confirmada | export/import JSON ou publicação confirmada |
| Controle da aba caiu | inventário e documentação do browser atual | preservar sessão; reconectar controle, sem reiniciar Chrome |

## Critérios de aceitação do aplicativo

- Todas as páginas do escopo foram coletadas ou têm lacuna identificada.
- Nenhuma linha anônima foi atribuída por posição/contagem.
- Importar novamente o mesmo arquivo com os mesmos metadados não duplica leituras; mudar horário do mesmo arquivo é rejeitado.
- Ausente e zero permanecem diferentes; correções negativas de contagens sobrevivem.
- Totais usam uma última leitura por campo, sem somar snapshots.
- Comparação usa o mesmo conjunto e os mesmos dois horários globais.
- Rotação, arquivo e nicho não são inferidos de performance.
- Páginas sem histórico permanecem no inventário, com cobertura explícita.
- Banco de postagens e agendamentos ficam íntegros; histórico não é apagado.
- Snapshot permitido não contém runtime, credenciais, administradores ou evidências privadas.
- Navegador ao vivo mostra a coleta nova; commit e arquivos servidos têm os mesmos hashes.

## Mudanças de interface

Adapters baseados em rótulos podem precisar de manutenção quando a Meta mudar. Observe o DOM/estado atual antes de alterar seletores. Nunca repetir uma transmissão cujo resultado ainda é incerto. Separe status singular/plural de exportações e revalide elementos depois de mudanças na tela.

## Escala do histórico

No início, JSON com histórico embutido pode bastar. Conforme crescer, meça tamanho, tempo de carregamento e paginação. Um caminho de evolução é publicar um resumo pequeno e arquivos de histórico por página, mantendo o banco canônico local. Não cortar histórico silenciosamente para fazer a página carregar.

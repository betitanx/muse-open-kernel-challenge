# Resumo da competição

## H223+H225 — resultado terminal — 04/11/2026

Check `ed262a767d294a45` concluído: v235 correta nos nove testes/30 formatos, sem flags, mas **1,403022169×**, IC95 [1,381976836;1,424067501]. Unpooler256 executou o kernel novo e mediu **71,35 µs ajustados**, contra 23,07 µs publicados; as outras 29 assinaturas permaneceram iguais. As 16 duplas privadas deram GM base/nova de 0,371010. Arquivar sem promover, publicar ou repetir automaticamente. **V228 permanece publicada e intacta em 1,514866388×**.

Dois contratos distintos foram revisados em PTX/CUTLASS. A única emissão integrada retirou exatamente cinco quartetos de fences, preservando publicação dos dados, bootstrap, rendezvous e matemática. R156, stack/local/spills zero, 52 recusas ABI reais e composição incremental aprovadas. Nenhuma ablação temporal separa H223 de H225.

API: **341º de 1722, 33 checks e 19 pontuadas disponíveis**, sem pendentes. Histórico: 106 avaliações, 102 jobs de check/quatro pontuadas, 101 códigos; 97 aprovações técnicas integrais de 93 códigos. Elegibilidade é separada. Total 146 entradas AOT; nesta retomada, uma AOT e um check, zero pontuadas. A publicação mantém 25 rotas próprias/cinco Lt; a candidata arquivada executou 26/quatro.

Próximas duas investigações em leitura: explicar o custo restante e auditar eventual sobreposição WGMMA com dois slots, sem supor wait1 suficiente. H224 em reserva; nenhuma nova fonte ou check selecionado. Estado e inventário: `resultados/estado_terminal_h223_h225_v235_20261104.json`. Os estados abaixo são históricos.


Atualizado em 04/11/2026 após a recuperação da v228 e os resultados H218 a H222 e H223+H225. Problema: `muse_bf16_proj`, projeções isoladas de Muse Spark Open em uma H200. Não mede o desempenho de serving do modelo inteiro.

## Posição e elegibilidade

| Item | Situação atual |
|---|---|
| Conta | betitanx |
| Posição no problema | 341º de 1.722 |
| Melhor publicação atual no placar provisório | v228, 1,514866388× sobre SGLang |
| Quinto colocado | reyanshsolis, 1,6835× |
| Líder | gogowhy, 1,7175× |
| Checks disponíveis hoje | 33 de 40 |
| Submissões pontuadas disponíveis hoje | 19 de 20 |
| Jobs pendentes | Nenhum nos 50 recentes |
| Encerramento de kernels | 05/11/2026, 05h de Brasília, 08h UTC |
| Abertura prevista de serving | 10/11/2026, 14h de Brasília, 17h UTC |

A v132 foi desclassificada por conter código-fonte comprimido ou codificado que é decodificado durante a execução. O status do job `d546f26fcfc945e3` continua `done`, com aprovação numérica, 1,540954514× e IC95 [1,517840196; 1,564068832], mas também informa `disqualified=true`. O resultado histórico não vale como classificação atual. Não há evidência nesta resposta de que o produto numérico tenha falhado; o motivo informado é a representação do código-fonte.

Os registros anteriores que chamavam a v132 de melhor publicação válida estão superados. Recuperamos suas implementações em fontes legíveis: a v228 compartilha sete templates explícitos, preservando byte a byte os fontes CUDA/C++ e mantendo o restante da AST Python. O check `7d5cb71b90884dc1` mediu 1,521547494×; a publicação `0311273f05d64c62` confirmou 1,514866388×, IC95 [1,492143392; 1,537589383], nove testes/30 formatos sem flags. A linha do placar foi confirmada e `submission.py` promovida aos bytes dessa candidata, SHA `a7146e05bc18ab9a70043d07e21171ddc5284724d69245508ede338b5eac7d8c`. Elegibilidade e classificação permanecem sujeitas à revisão final da organização.

## O trabalho realizado

1. Instalamos o OKC em WSL dedicado; o usuário realizou o login e a aceitação das regras. Inicializamos o problema e preservamos os arquivos oficiais.
2. Criamos contexto local para H200/SM90 e Muse Spark Open, laboratório, arquivo de candidatas, hashes, resultados e perfis. Organizamos três revisores fixos, com escritas e envios centralizados.
3. Na fase inicial, investigamos roteador FP32, split-K, GEMV, pesos transpostos, TinyGEMM e escolha de bibliotecas. A v4 foi publicada com 1,1635×; a composição v14 chegou a 1,2256×. Ambas foram superadas pela v228 legível, atualmente publicada em 1,5149×.
4. Exploramos geometrias, leitores, representação exata dos pesos, decodificação aritmética, pipelines, TMA, WGMMA, Gluon, epílogos, reduções e clusters. Compilações locais, inspeção dos nossos binários e modelos CPU ajudaram a descartar erros e custos antes dos checks; não comprovam velocidade H200.
5. Estudamos a referência MIT de Nathen, preservando atribuição. A reprodução v131 mediu aproximadamente 1,4954×; a modificação do decoder na v132 mediu 1,5283× no check e 1,5410× na publicação. Essa publicação foi posteriormente desclassificada pelo formato codificado do fonte herdado e mantido.
6. As avaliações seguintes de variantes e combinações não comprovaram uma substituta superior à medição histórica da v132. Incluíram regressões, falhas e resultados inconclusivos, todos preservados. Exemplos recentes: duas cadeias no pooler, H213, mediram 1,5082×; inversão B8, H215, mediu 1,5111× e regrediu nas duas rotas privadas comparadas.
7. Recuperamos a H218 em fonte legível, v229, e executamos o primeiro check H200: passou na correção, mas regrediu para 1,3882×. A H219/v230, multicast de X no QKV128, também passou integralmente e regrediu para 1,3674×. Os perfis confirmaram que os kernels novos executaram. Ambas foram arquivadas sem publicação; a v228 foi preservada.
8. H220 reduziu operações remotas com expectativa local e ACK agregado após barreira dos consumidores. Um novo modelo passou em 120 ordens/31 negativos; uma compilação integrada e 32 recusas nativas de ABI passaram. A primeira composição local, v231, foi recusada por hashes dos arquivos CRLF incompatíveis com seus literais LF. A v232 corrigiu somente os hashes, passou no controle incremental e no check H200, mas regrediu para 1,3833×. Foi arquivada sem publicação.
9. H221 reduziu o anel para três estágios, com 98.368 B de memória compartilhada. A API H200 passou a prever duas CTAs por SM e 39 clusters para a grade de 18. A v233 passou integralmente, mas mediu 1,3853×; aumentar a capacidade prevista não recuperou o desempenho da publicação. Foi arquivada sem publicar. Os novos custos incluem menos antecipação, módulo três e mais liberações de slots.
10. H222 alterou somente o ACK remoto para `release.cta`, seguindo o padrão oficial CUTLASS e preservando as esperas e as demais barreiras. A emissão retirou quatro fences desse caminho, mantendo R116 e zero spills. O primeiro job teve falha de infraestrutura e foi reembolsado; a retentativa com bytes idênticos passou integralmente em **1,4221×**. QKV128 mediu 41,25 µs ajustados, ainda acima dos 13,91 µs publicados. A candidata foi arquivada sem publicação.
11. A nova evidência fundamenta auditar o ACK de lifetime da H218, preservando os sinais que transportam W/DSM e armam READY. Outra investigação verifica se há evidência de colocação por SM e quais garantias as políticas de clusters realmente fornecem. Ainda não há nova candidata selecionada. O repositório Nathen não recebeu atualizações desde o pin incorporado.

## Quanto foi realmente avaliado

O inventário consolidado até 04/11 tem **106 jobs oficiais: 102 de check e quatro pontuados, de 101 códigos distintos**. Há 97 avaliações integrais aprovadas nos testes, correspondentes a 93 códigos distintos, e nove jobs não integrais. Um desses nove foi a falha de infraestrutura reembolsada da H222, sem resultado técnico; a retentativa usa o mesmo código. Esses números descrevem aprovação técnica histórica, não elegibilidade após revisão da organização ou quantidade de checks cobrados.

O acervo anterior de 231 experimentos e 237 fontes primárias recebeu nove candidatas locais novas: v227, recusada por tamanho sem job; v228, avaliada e publicada; v229 e v230, corretas e regressivas; v231, recusada localmente por hashes inconsistentes; v232, v233 e v234, corretas e regressivas. Inclui versões substituídas, controles, rejeitadas e pendentes; não são soluções independentes prontas para GPU. Existem 146 entradas nativas compiladas antecipadamente no inventário local; compilação não equivale a validação GPU. A API registra sete checks cobrados e uma publicação pontuada no dia 04/11, além de uma tentativa reembolsada.

## Pendências e limites

- Não alcançamos top 5 ou primeiro lugar.
- A base principal foi recuperada como v228 legível e publicada. Derivadas antigas com fontes codificadas não devem ser enviadas como estão. Uma nova representação legível exige revisão completa e check; remover codificação não garante aceitação final.
- H218/v229, H219/v230, H220/v232, H221/v233 e H222/v234 passaram na correção e executaram na H200, mas perderam desempenho frente à publicação. As próximas investigações ainda não têm candidata selecionada.
- A v228 confirmou 25 rotas nativas e cinco em cuBLASLt. O objetivo nunca foi eliminar bibliotecas por si só, e seus caminhos não devem ser anunciados como kernels próprios.
- Mantivemos como critérios precisão, entradas novas, grafos CUDA, L2 frio e carga integral. A ausência de flags automáticas não impediu a desclassificação posterior; elegibilidade precisa ser verificada separadamente.

## Evidências

- Estado vigente, placar, cotas e inventário: `muse_bf16_proj/resultados/estado_terminal_h222_v234_20261104.json`.
- Publicação legível: `muse_bf16_proj/resultados/estado_publicacao_legivel_v228_20261104.json`.
- H218 regressiva: `muse_bf16_proj/resultados/estado_terminal_h218_legivel_v229_20261104.json`.
- Próximas pesquisas: `muse_bf16_proj/resultados/plano_h220_h221_apos_h219_20261104.json`.
- Fonte H221/ST3 preparada: `muse_bf16_proj/resultados/fonte_fixa_h221_ST3_20261104.json`.
- Fonte H222 e contrato de ordenação: `muse_bf16_proj/resultados/fonte_fixa_h222_release_cta_20261104.json`.
- Comparação da nova publicação com a v14: `muse_bf16_proj/resultados/comparacao_publicacao_legivel_v228_com_v14_20261104.json`.
- As consultas abaixo preservam o retrato anterior à recuperação.

- Placar atual: `muse_bf16_proj/resultados/resumo_competicao_20261104_1793806095208_placar.json`.
- Cotas: `muse_bf16_proj/resultados/resumo_competicao_20261104_1793806095208_problemas.json`.
- Calendário: `muse_bf16_proj/resultados/resumo_competicao_20261104_1793806095208_etapa.json`.
- Jobs: `muse_bf16_proj/resultados/resumo_competicao_20261104_1793806095208_jobs.json`.
- Status consultado da v132: `muse_bf16_proj/resultados/d546f26fcfc945e3.json`.
- Inventário histórico: `muse_bf16_proj/resultados/inventario_final_orientacao_n8_h215_v225_20261027.json`.
- Registro H218: `muse_bf16_proj/resultados/registro_integridade_integracao_compartilhada_h218_v226_20261027.json`.
- Histórico detalhado: `lab.md` e `muse_bf16_proj/DESEMPENHO.md`.

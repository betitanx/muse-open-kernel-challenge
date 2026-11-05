# Resumo da competição — 05/11/2026

Atualizado em 2026-11-05T08:01:31.945Z (05/11/2026, após 05h, Brasília). [Placar oficial do problema](https://openkernelchallenge.dev/problems/muse_bf16_proj).

## Situação atual

| Item | Estado confirmado |
|---|---|
| Posição provisória em muse_bf16_proj | 192º |
| Melhor publicação válida | **1,574877619×** |
| Intervalo oficial de 95% | [1,551254455; 1,598500783] |
| Checks usados hoje UTC | **18 de 40** |
| Saldo de checks ao encerrar | **22; novos envios fechados** |
| Pontuadas usadas hoje UTC | **1 de 20** |
| Saldo de pontuadas ao encerrar | **19; novos envios fechados** |
| Jobs pendentes | Nenhum na última consulta |
| Etapa de kernels | **Encerrada em 05/11 às 08h UTC / 05h de Brasília** |

Foram quinze checks e uma pontuada desde a solicitação das últimas horas; quatro checks corretos nesta continuação. Não há espera obrigatória entre avaliações durante o trabalho ativo; usamos as cotas reais da API.

## Publicação preservada

A v245, job af7157ee60b0482f, está em muse_bf16_proj/submission.py. É a reprodução legível da solução pública keell, com atribuições e licenças preservadas. O check da mesma fonte marcou 1,586848975×; a pontuada marcou **1,574877619×**, passando nove testes, 30 benchmarks e 30 perfis sem flags.

Melhorou **3,9615%** sobre a publicação válida anterior v228, de 1,514866388×, com intervalos separados. Mantém quatro projeções BF16 M256 em cuBLASLt; as demais 26 rotas usam os kernels das implementações integradas. O resultado mede projeções isoladas, sem alegar o mesmo ganho no modelo inteiro.

Fonte: muse_bf16_proj/experimentos/reproducao_publica_keell_h235_v245.py.

SHA-256: 7328887dee1bbaf4e13a797acddb2f515448a5c1920f5704a63fc8c38f14201a.

Uma publicação antiga de 1,540954514× foi desclassificada por conter fonte comprimida decodificada em runtime. Permanece no histórico, sem ser tratada como resultado válido ou piso de comparação.

## Últimos checks

| Hipótese | Resultado global | Decisão |
|---|---:|---|
| H239: prefixos pooler32/unpooler32 | 1,578602945× | Correta; sinal exploratório no pooler, sem ganho global demonstrado. |
| H240: EARLY nas três RQ M128 | 1,576321438× | Correta; novos kernels executaram, sem ganho global. |
| H241: prefixos somente QKV16 | 1,574162075× | Correta; não superou consistentemente as duas referências da v245. |
| H238/v253: consulta Lt o256 | 1,595119240× | As alternativas eram muito mais lentas; o pin publicado foi mantido. A alta nominal não demonstra ganho da mudança. |
| H242: QKV16 R16/ST3/NESC4 | 1,568025345× | Correta; capacidade calculada de dois blocos por SM confirmada, mas QKV16 ficou mais lenta. |
| H243/v256: consulta Lt alinhada | 1,546141538× | Filtro recusou quatro alternativas antes da medição; pin mantido. |
| H243/v257: filtro compatível com o pin | 1,570765181× | Três alternativas corretas perderam os 16 blocos cada; pin mantido. |
| H245/v258: router QS8 | 1,582186524× | Executou corretamente, mas não superou consistentemente as duas referências. |
| H246/v259: constantes M128/S16 | 1,572022129× | Executou corretamente; SASS menor não trouxe ganho demonstrado. |

Nenhuma dessas variantes foi promovida ou enviada como pontuada. Fontes, hashes, jobs, intervalos, regressões e decisões estão em [lab.md](lab.md). Combinações exigem validação integral; pontuada não substitui check.

## Inventário e trabalho realizado

**126 avaliações oficiais: 121 checks e cinco pontuadas**, cobrindo 120 hashes de código distintos. Foram 116 aprovações técnicas integrais de 111 códigos; dez avaliações não integrais e uma desclassificação conhecida. Aprovação técnica não equivale a elegibilidade.

As frentes incluíram GEMV/GEMM em Triton e CUDA, vetorização, codecs sem perda de BF16, correções de escapes, TMA, MMA/WGMMA, pipelines, redução, layouts, roteador FP32 com hi/lo permitido e planos de bibliotecas. Reservas locais são propostas, sem ganho H200 presumido.

O inventário recente de emissão nativa tem **186 entradas AOT**, incluindo controles. É distinto de execuções oficiais na H200. Modelos CPU e contagens SASS não comprovam velocidade.

Arquivos oficiais e a melhor publicação foram preservados. Não se alteraram tolerâncias, carga, entradas, grafos CUDA, protocolo frio ou avaliador.

## Encerramento

A API confirmou a etapa fechada após 08h UTC / 05h de Brasília. Não há jobs pendentes. Não iniciar novos testes ou submissões. A melhor publicação de **1,574877619×** permanece intacta em **192º, provisoriamente**; a meta top 5 não foi atingida. Foram quinze checks e uma pontuada na reta final, incluindo quatro checks corretos na última continuação. Nenhuma variante posterior demonstrou ganho suficiente para substituir a publicação.

[Estado canônico de encerramento](muse_bf16_proj/resultados/estado_encerramento_20261105.json). [Auditoria das melhores](muse_bf16_proj/resultados/auditoria_melhores_e_expansao_router_20261105.json).

## Revisão pública final

Em 05/11, às 07h58 UTC, as origens públicas de Nathen e keell continuavam nos mesmos commits já analisados. A triagem adicional não qualificou uma nova candidata integral. Fontes, hashes, escopo dos relatos externos e decisões estão no [manifesto da pesquisa](muse_bf16_proj/resultados/pesquisa_final_fontes_publicas_20261105.json). Zero novos checks ou pontuadas nessa revisão.

## Estado da meta

Três confirmações consecutivas pela API demonstraram o mesmo impedimento: etapa fechada, sem jobs pendentes e top 5 não atingido. A auditoria qualificou o registro da meta como bloqueada, sem tratá-la como concluída. Não foram feitos novos testes ou submissões. [Auditoria](muse_bf16_proj/resultados/auditoria_bloqueio_3_20261105.json).

# Retomada da otimização

**Décimo primeiro check concluído: v128/H117 passou 9 + 30 e confirmou Stream-K, mas perdeu nas três rotas.** V124 continua maior ponto correto, v14 publicada. API: 19/20 checks usados, um disponível; zero scored. Nenhum pendente. H118/MemPool nativo é a única pesquisa ativa; os blocos antigos abaixo são históricos até a consolidação.

**V124 é o maior ponto integral validado: 1,2494612800513463×, IC95 [1,230719360850576; 1,2682031992521163].** [Job 7f0205c317cb4ca0](resultados/estado_terminal_check_v124_cota_api.json), nove testes e 30 formatos aprovados, flags vazias. A publicação v14 permanece 1,22560789754027×, IC95 [1,2072237790771658; 1,243992016003374]. Os intervalos sobrepõem; não promover, não publicar e não usar scored como teste adicional.

`submission.py` e `experimentos/combinado_v14.py` continuam com SHA raw `1ed5c8403f907ed81c28356966f1e527a5a8b6ae5c206b1f63965d76b868e0ea`. V124 está arquivada em [combinado_qkv_medio_v124.py](experimentos/combinado_qkv_medio_v124.py), SHA `b0c60edc79923b068304049e92f2308cdcf514a7008e2e32a0ed4b0786419aa4`. A meta top 5, primeiro lugar e manutenção continua ativa, ainda não alcançada.

## Cotas e próxima decisão

O usuário pediu utilizar os checks disponíveis em 24/10. Os limites locais de oito por dia e dois por rodada foram retirados; usar as cotas reais da API. [Consulta após v124](resultados/consulta_pos_v125_cota_api_problemas.json), às 21h43 UTC: **16 checks usados e quatro disponíveis, zero scored usados e dez disponíveis**. [Todos os 38 jobs estão terminais](resultados/consulta_pos_v125_cota_api_jobs.json). As seis horas do agendamento são apenas a frequência de retomada automática, sem intervalo obrigatório durante trabalho ativo.

Ler a [seleção após oito checks](resultados/selecao_pos_oito_checks_cota_api.json), o [inventário integral](resultados/revisao_resultados_validos_pos_v125_cota_api.json), [PENDENTES.md](PENDENTES.md), [lab.md](../lab.md) e a [política vigente](resultados/politica_cotas_api_20261024.json). Há 35 integrais corretos e três não integrais; nenhum ganho com IC separado. Nenhum nono check foi escolhido nesse snapshot. Selecionar uma nova fonte somente após identificar causa concreta e revisar as evidências mais recentes; não percorrer automaticamente a fila ou repetir fontes superadas para consumir saldo.

Antes de cada envio, consultar etapa, cotas, jobs e placar com `okc.ps1`; acompanhar qualquer job pendente antes de iniciar outro. Salvar o resultado com `consultar_resultado.ps1`. Imediatamente antes de publicar, comparar todos os resultados válidos e enviar somente a melhor combinação integralmente validada com ganho além da incerteza da publicação. Continuar análise, implementação e controles locais úteis mesmo sem checks.

## Oito checks desta solicitação

| Versão | Job | Aceleração | IC95 | Decisão |
|---|---|---|---|---|
| v111 | `6f6076a3d4614cdc` | 1,108199× | [1,091576; 1,124822] | Regressão; não repetir sem causa nova |
| v103 | `8815fd1447884258` | 1,045648× | [1,029963; 1,061332] | Regressão; não repetir sem causa nova |
| v112 | `89a183c93dd74533` | 0,935241× | [0,921212; 0,949269] | Regressão; não repetir sem causa nova |
| v110 | `76b9b573567d4ea1` | 1,173544× | [1,155941; 1,191147] | Regressão; não repetir sem causa nova |
| v93 | `3e4e7aeecd5c44c8` | 1,003953× | [0,988893; 1,019012] | Regressão; não repetir sem causa nova |
| v122 | `d3136f837d8048a0` | 1,225351× | [1,206970; 1,243731] | Dentro da incerteza; não integrar automaticamente |
| v124 | `7f0205c317cb4ca0` | 1,249461× | [1,230719; 1,268203] | Melhor ponto; IC sobreposto, guardar |
| v125 | `f2009112f18a4ff1` | 1,205642× | [1,187557; 1,223726] | Quatro M1 regressivas frente à v124; rejeitar |

Todos passaram em nove testes e 30 formatos, sem flags; nenhum scored foi usado. Os relatórios individuais preservam SHA raw, hipótese, seleção anterior ao envio, perfis, regressões e decisão. A composição v124 incorpora o sinal qkv M128 da v110 na v101 sem supor soma de ganhos.

V125/H115 investigou uma orientação ainda não exercida do GEMM pela API mv. O [check integral](resultados/estado_terminal_check_v125_cota_api.json) passou, mas custou 1,656% mais que a publicação e 3,635% mais que a v124. Os quatro perfis M1 são nativos; três contêm redução split-K interna. Uma chamada Python não garante um único kernel. Todas as quatro rotas pioraram frente à v124, sem componente favorável para recortar; não repetir a fonte sem mecanismo novo.

## Melhor composição e limites

V124 preserva literalmente o prefixo v101 e acrescenta somente qkv de 65 a 128 tokens por BMM da biblioteca, parciais FP32 e redutor original. Entre os 30 formatos, 14 rotas diferem da publicação e 16 são preservadas. Os perfis confirmam três GEMVs M1 herdadas, cinco redutores BF16 em registradores, cinco redutores do router em registradores e BMM com redutor original no qkv M128. Produtos maiores e parte dos demais caminhos continuam nas bibliotecas; nomes de kernel não comprovam API host específica ou SHA do binário remoto.

O ponto implica +1,946% em aceleração e −1,909% em custo agregado frente à publicação, sem ganho comprovado além da incerteza. Router M16 tem latência bruta +5,47% e razão normalizada −3,95%; pooler M256 tem +9,02% e −3,99%. Pooler M256 usa o caminho preservado, portanto não atribuir sua oscilação à integração. Comparações entre jobs são independentes, sem IC pareado por formato. Os vizinhos M65…127 não têm medição individual.

V122 exercitou as três GEMVs com decoder inteiro sem perda. Qkv/o perderam; unpooler M1 foi neutro frente à v101. Não acrescentar o decoder à melhor composição por comparação apenas com v14. V123 guarda o recorte isolado, cujo mesmo cálculo já foi validado na v124; outro check isolado exige uma dúvida decisiva nova.

## Pesquisa arquivada e retomada

H112 criou v120, v121 e v122. V120 tem 13 BAR e 8 KiB shared nas duas sondas, sem prioridade remota. V121 falhou em duas AOT pela ausência de `gl.broadcast_to`; v122 corrige só a máscara. Seis AOT v122 passaram sem spills. O controle K2048/BN8 tem três BAR na redução, R64 e 128 B shared; as conversões do leitor são triviais e somente thread0 escreve oito BF16 por CTA. Uma BAR vale apenas para as outras sondas que a demonstraram.

O [modelo CPU v122](resultados/verificacao_gemv_planos_gluon_v122.json) conferiu 148.480 valores BF16, seis produtos novos com L2 máximo de 0,207014%, sondas exatas, host simulado e fronteiras. Não executa layouts, lanes, associação GPU ou captura. A [errata](resultados/errata_verificacao_gemv_planos_gluon_v122.json) esclarece o rótulo de erro por elemento sem modificar o relatório usado na seleção.

V92/BK128 RS e v119/BK128 WS ficam em reserva com baixa prioridade; resultados negativos das famílias exigem mecanismo novo antes de ampliar ou avaliar. ILC/owner, heurística de biblioteca e Tiny4 via NVRTC em processo permanecem pesquisa. H116 precisa fechar dependências, ABI TMA, contexto, prazo e captura; não há candidata nova pronta ou nono check escolhido. Acervo: 119 fontes congeladas, 109 versões e 870 AOT compiladas; duas falhas novas v121 separadas dos sucessos. Os três agentes fixos trabalham em leitura; principal centraliza escrita, compilação e OKC.

Último [placar](resultados/consulta_pos_v125_cota_api_placar.json): 281º/816, corte top 5 em 1,5186×, líder em 1,6099×. [Etapa](resultados/consulta_pos_v125_cota_api_etapa.json) aberta até 05/11/2026 às 08h UTC. Ambiente observado H200/132 SM, Torch 2.14.1+cu130, Triton 3.8.0 e FlashInfer 0.7.0.post1. Preservar oficiais, precisão, entradas novas, carga, CUDA graphs e L2 frio; não modificar nem inspecionar internamente o avaliador.

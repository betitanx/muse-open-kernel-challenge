# Continuidade da otimização

**Estado vigente 25/10, 00h39 UTC:** v138 está no quarto check do dia, job `532716c511ac4632`; acompanhar o mesmo ID. V135 passou9+30,1,523733×/ICsobreposto, sem ganho sobre132. Melhor integral132:1,5283491356×, IC95 [1,5054238985;1,5512743726]. Zero scored; publicação14preservada até comparar138. H122é a hipótese ativa. Os estados anteriores abaixo são históricos.

**Estado vigente 25/10, 00h20 UTC:** v132 é a melhor integral: 1,5283491356×, IC95 [1,5054238985; 1,5512743726], 9+30 sem flags. V135 integrada está no terceiro check do dia: `9991824664e44f3f`; acompanhar o mesmo ID. Zero scored; publicação v14 preservada até comparar as melhores. H122/poolerM128 DSM foi pré-registrada para pesquisa local. H121/H122 são as duas hipóteses ativas. Estados anteriores abaixo são históricos.

**Atualização 25/10, 00h07 UTC:** v131 terminou integralmente correta em 1,4953687045×, IC95 [1,4729381739; 1,5177992351]; é a nova melhor base própria, superior à publicação com IC separado. Lt130101, sete workspaces efetivos zero, C12 confirmado. V132 agora está em avaliação: job `b0d6dd2cdc6743a7`. Acompanhar esse ID; não duplicar. Dois checks novos do dia e zero scored. Publicação v14 preservada enquanto comparamos as melhores variantes. Os estados anteriores abaixo são históricos.

**25/10/2026, 00h01 UTC:** check H119/v131 `616b13c232624597` iniciado e queued. Acompanhar com consultar_resultado.ps1; não duplicar. API renovou 20 checks/dez scored; um check novo e zero scored, saldo esperado 19 sujeito à consulta. [Seleção prévia](resultados/selecao_pre_check_v131_20261025.json) conferiu os 38 integrais próprios. Publicação v14 preservada. Os estados de cota/ausência de pendentes de 24/10 abaixo são históricos.

Complemento H120: [v133 somente pooler](experimentos/decoder_aritmetico_pooler_v133.py), SHA `a2d8bd8a5fc6ffb30925ccbf0edf8982d593215be6fd1b8ada8ffd3b89aa37ee`, foi salva com controles CPU aprovados e três revisões concretas aprovadas. Preserva v131 raw e todos os helpers/globals originais; usa módulo, planos e caches próprios. Devicebody idêntico ao AOT pooler aritmético após renomeação, sem nova compilação ou produto. Falta medir build/preparo adicional, captura, correção e tempo H200. Acervo atualizado: 127 fontes históricas/117 versões, mais sete nativas separadas. V131 segue preferência condicional; v132 e v133 são alternativas da mesma H120, sem fila automática.

A calculadora CPU SDK13 indica teto teórico o M8 de cinco para quatro CTAs/SM com R48→50. A grade tem 512 CTAs e ainda cabe em 4×132=528; não há nova onda ou regressão demonstrada. Pooler mantém um CTA/SM. Não escolher v133 apenas por esse teto.

Estado de 24/10/2026 após os doze checks e a pesquisa H119/H120, sem nova avaliação remota. A meta top 5, primeiro lugar e manutenção continua ativa. Todos os jobs estão terminais; nenhum resultado pendente. [Seleção completa](resultados/selecao_pos_doze_checks_cota_api.json), [laboratório](../lab.md), [inventário](resultados/revisao_resultados_validos_pos_v129_cota_api.json).

## Referência MIT e preferência atual

A [análise da referência](../referencias/referencia-nathen-h119-h120-2026-10-24.md) fixa o commit86aff75 e confere o resultado externo de1,51857×, sem tratá-lo como check desta conta. [Seleção atual](resultados/selecao_referencia_nathen_h119_h120_20261024.json): v131 é preferência condicional para reproduzir a composição e confirmar versão/workspaces/rotas; v132 é reserva do decoder, a reavaliar após nova evidência. Nenhum check escolhido automaticamente. V130 foi rebaixada.

[V131](experimentos/referencia_nathen_v18_v131.py) preserva a fonte raw, MIT integral e acrescenta apenas diagnóstico do prepare. [V132](experimentos/decoder_aritmetico_nathen_v132.py) troca coerentemente pack/unpack/tabela/decoder em duas rotas. [Controles CPU](resultados/verificacao_referencia_nathen_h119_h120.json) passaram em138 casos/577.536 valores por codec, sem GEMM/GPU. Quatro [AOT nativas](resultados/aot_c12_h120_nvrtc13/resumo.json) passaram: pooler336→312/R58 igual; o432→384/R48→50, zero spills. São emissão e modelos, sem ganho medido.

Confirmar Lt130101 e workspace efetivo dos cinco caminhos finais. 128MiB é capacidade; scratch positivo compartilhado exige prova ou correção validada de segurança concorrente antes da promoção. Preservar as guardas, flags, precisão, escapes e atribuição. Resultado do autor não autoriza publicar código sem validação integral própria.

## Melhor resultado e publicação

A [v124](experimentos/combinado_qkv_medio_v124.py), SHA `b0c60edc79923b068304049e92f2308cdcf514a7008e2e32a0ed4b0786419aa4`, job `7f0205c317cb4ca0`, passou nove testes e 30 formatos sem flags: 1,2494612800513463×, IC95 [1,230719360850576; 1,2682031992521163]. É o maior ponto integral correto. Não publicar enquanto o ganho sobre v14 permanecer dentro da incerteza.

A [publicação v14](submission.py) permanece SHA `1ed5c8403f907ed81c28356966f1e527a5a8b6ae5c206b1f63965d76b868e0ea`, job `803cbbd3eee447fa`: 1,22560789754027×, IC95 [1,2072237790771658; 1,243992016003374]. Nenhum scored novo. Preservar os cinco arquivos oficiais.

## Cotas e retomada

API: 20/20 checks usados, zero disponíveis neste dia UTC; oito usos anteriores mais doze desta solicitação. Zero de dez scored usados. Não usar scored como check extra. [Consulta final](resultados/consulta_pos_v129_cota_api_problemas.json), [42 jobs](resultados/consulta_pos_v129_cota_api_jobs.json), [etapa](resultados/consulta_pos_v129_cota_api_etapa.json), [placar](resultados/consulta_pos_v129_cota_api_placar.json).

Os antigos tetos de oito por dia e dois por rodada foram retirados. As seis horas limitam apenas a frequência da retomada automática; não há espera obrigatória entre avaliações durante trabalho ativo. Usar cotas reais e selecionar uma avaliação de cada vez, depois da evidência anterior. Nenhum próximo check foi escolhido. Reconsultar API e rever todas as candidatas antes de enviar. Continuar pesquisa local útil mesmo sem checks; preservar o que precisa de validação futura.

## Resultado desta sequência

Onze dos doze checks passaram integralmente; v126 falhou em build CCCL e sua correção v127 passou. V124 elevou o maior ponto. V122 ficou sem ganho integral demonstrado; v110 deu o sinal qkvM128 depois integrado e validado na v124. V111/Lt, v103/orientação, v112/cooperativa, v93/RS e v125/mv perderam. Não repetir essas fontes por fila ou contagens de recursos.

[Tiny4 v127](resultados/estado_terminal_check_v127_cota_api.json): 1,237246×, perfis confirmaram oM1/M8, ambas piores que v124. Não ampliar K por shared menor. [Stream-K v128](resultados/estado_terminal_check_v128_cota_api.json): 1,159932×, três rotas confirmadas, todas regressivas; agregado +7,7185% com IC separado desfavorável. Preservar reset/workspace e não variar estágios sem causa nova.

[ILC v129](resultados/estado_terminal_check_v129_cota_api.json): 1,238432×, IC95 [1,219855; 1,257008], nove testes/30 formatos/sem flags, preparo17,354s. Log próprio confirma oito segmentos GENERIC e nove representações/360MiB lógicos no retorno do prepare. São364MiB solicitados/arredondados em segmentos, não compressão física medida. Agregado +0,890586% frente à v124, IC sobreposto. Não promover, não inferir clonagem ou incompressibilidade. Os mesmos cálculos/bibliotecas v124 foram preservados; 30/30 perfis mantiveram nomes e contagens.

## Trabalho salvo

[V130](experimentos/ilc_cobertura_forward_v130.py), SHA `413f559713b22a34f4cda705049c7171329d9dc7630478274607ca5611890bcf`, é diagnóstico condicional sobre v129 literal. Em cada invocação Python exige cobertura GENERIC dos nove pesos recebidos antes do calculador. Sem cache de fase/ponteiros, valores de X/Y, cópia, reparo ou fallback. Lê de X somente o dispositivo. Falha identifica cobertura ausente, sem identificar responsável ou método externo; sucesso não mede compressão/HBM. Python roda na invocação/captura, sem repetição em cada replay.

[Controles v2](resultados/verificacao_cobertura_ilc_v130_v2.json): quatro forwards, dez falhas, nove clones individuais, proprietário ausente e imutabilidade passaram. [Errata preservada](resultados/errata_controle_forward_ilc_v130.json) corrige somente a lambda do teste, sem mudar candidata ou somar casos repetidos. [Três revisões](resultados/revisao_agentes_cobertura_ilc_v130.json) sem bloqueio. Falta validação remota, sem envio automático. Antes de escolher esse controle, confirmar que a dúvida ainda muda a decisão perante outras candidatas.

V119/WS e v92/RS são reservas condicionais, não próximas avaliações. Recursos/SASS não comprovam ganho, e mecanismos próximos regrediram. V123 não necessita check isolado automático: componente já validado na v124. Consultar [PENDENTES.md](PENDENTES.md).

## Integridade e acervo

126 fontes históricas/116 versões, mais sete arquivos nativos candidatos anteriores separados; 870 AOT Triton, duas Tiny e quatro C12 nativas separadas. Seis falhas Triton e quatro falhas de configuração NVRTC rastreadas ficam fora dos sucessos. 42 produtos CPU novos na fase; nenhum produto novo em H118. Core/plugin real H118 passou 52 controles com driver falso e ELF CPU, sem CUDA; preparo Python passou dois estados/18 cópias/11 falhas/quatro builders simulados. Não repetir suítes aprovadas sem mudança ou lacuna concreta.

Todos os candidatos, jobs, SHAs, correção, intervalos e decisões estão no laboratório. Usar consultar_resultado.ps1 para o resultado de qualquer novo job. Preservar precisão, inputs novos, grafos, L2 frio e caminhos de bibliotecas. Não modificar nem inspecionar internamente o avaliador. Reutilizar somente os três agentes fixos gpt-6.1-sol/xhigh em leitura; principal centraliza escrita, seleção e envios.

Na última consulta, posição293º/844, top5 em1,5331×, líder1,6099×. Kernels abertos até05/11/2026 às08hUTC. A automação continua ativa a cada seis horas, sem notificações quando não há mudança relevante. [Histórico anterior à consolidação](CONTINUIDADE_HISTORICO_ATE_V128_20261024.md) contém snapshots cujas cotas, janelas e prioridades antigas foram substituídas pela seleção vigente.

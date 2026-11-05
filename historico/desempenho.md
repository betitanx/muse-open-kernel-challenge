# Registro de desempenho

Relatório histórico das primeiras rodadas. O registro principal das próximas experiências é [lab.md](../lab.md), com as skills específicas, o estado de cotas e a retomada autorizada. As posições e cotas narradas abaixo correspondem às consultas indicadas em cada trecho.

Problema: `muse_bf16_proj`. As medições oficiais comparam cada candidato com o SGLang na mesma H200, alternando execuções de grafos CUDA com cache L2 frio. Os arquivos oficiais `TASK.md`, `AGENTS.md`, `baseline.py`, `reference.py` e `checks.py` foram preservados.

| Experimento | Execução | Testes | Aceleração sobre o SGLang | Intervalo de 95% | Decisão |
|---|---|---|---|---|---|
| PyTorch inicial | `fb8d3413c2154779` | 9/9 | 0,9020× | 0,8885–0,9156× | Referência inicial; não usar como solução final |
| Divisão paralela de K, versão 1 | `799432335b18489e` | 9/9 | 1,0513× | 1,0355–1,0671× | Supera o SGLang; perdas em várias projeções BF16 e no roteador de lote 256 |
| Roteador com blocos maiores, versão 2 | `13c705e40c874fcd` | 9/9 | 1,1314× | 1,1145–1,1484× | Melhora confirmada; projeções BF16 originais |
| Pesos transpostos, versão 3 | `874095ed89b84128` | 9/9 | 1,1588× | 1,1414–1,1762× | Melhora principalmente no `pooler`; algumas transposições não ajudam |
| Caminhos consolidados, versão 4 | `4f91b230fafe4069` | 9/9 | 1,1705× | 1,1530–1,1881× | Melhor versão validada; selecionada para submissão pontuada |
| Submissão pontuada da versão 4 | `19a230ce058a454d` | 9/9 | 1,1635× | 1,1461–1,1810× | Registrada no placar; versão final mantida |
| Projeções transpostas em blocos, versão 5 | `103023b0987a49ed` | 9/9 | 1,0844× | 1,0681–1,1006× | Rejeitada como conjunto; melhora apenas em parte dos lotes pequenos |
| Roteador intercalado, versão 6 | `3daf6c78310244e7` | 9/9 | 1,1807× | 1,1630–1,1984× | Ganhos no roteador pequeno; perda no lote 256; não submetida |
| GEMV BF16, versão 7 | `72fa71b3ec6c48d8` | 9/9 | 1,1942× | 1,1763–1,2121× | Melhora no lote de um token; candidata à combinação, não submetida |
| Transferências TMA, versão 8 | `f209d949116e4deb` | 9/9 | 1,0260× | 1,0107–1,0414× | Rejeitada; piora nos lotes grandes |
| TinyGEMM do FlashInfer, versão 9 | `b29f9e60d10e4c71` | 9/9 | 0,8724× | 0,8594–0,8855× | Rejeitada como conjunto; aproveitado apenas o `unpooler` de até oito tokens |
| TMA com blocos menores, versão 10 | `99bc185f3ce14a89` | 9/9 | 1,0063× | 0,9912–1,0214× | Rejeitada; piora nos lotes grandes |
| cuDNN, versão 11 | `61d928acd7b44c28` | 9/9 | 1,1374× | 1,1204–1,1545× | Rejeitada; sem ganho sobre a versão publicada |
| Roteador para lote maior, versão 12 | `24d5aca661bf4b26` | 9/9 | 1,1812× | 1,1635–1,1990× | Aproveitado na combinação; sem submissão isolada |
| cuBLASLt com autotuning, versão 13 | `d5e0aea953ce4ea3` | 9/9; falhou na captura | — | — | Rejeitada: inicia profiling durante captura de grafo CUDA em formato ainda não ajustado |
| Combinação dos ganhos, versão 14 | `17d3f526f0444b11` | 9/9 | 1,2391× | 1,2205–1,2577× | Melhora comprovada sobre a versão 4; selecionada para publicação |
| Submissão pontuada da versão 14 | `803cbbd3eee447fa` | 9/9 | 1,2256× | 1,2072–1,2440× | Ganho confirmado; `submission.py` corresponde a esta versão |
| Projeção direta, versão 15 | `8a8f7ba130c84d5d` | 9/9 | 0,9988× | 0,9839–1,0138× | Rejeitada; a retirada de split-K reduziu o paralelismo |
| cuBLASLt, segundo algoritmo, versão 16 | `76d2cee9b9a04058` | 9/9 | 1,1227× | 1,1059–1,1396× | Rejeitada; não supera a versão publicada |
| cuBLASLt, terceiro algoritmo, versão 17 | `32f34c88a5134e5f` | 9/9 | 1,1283× | 1,1114–1,1452× | Rejeitada; não supera a versão publicada |
| Especialização de warps, versão 18 | `aa88c1dad5ab4290` | Falha de compilação | — | — | Descritores TMA da CPU incompatíveis com o particionamento do compilador no Hopper |
| Descritores na GPU e execução persistente, versão 19 | `2719cdcc8d5e4572` | 9/9 | 1,0465× | 1,0308–1,0622× | Compilação corrigida; rejeitada por regressão nos lotes grandes |

A versão inicial perde principalmente no roteador dos lotes maiores: utiliza multiplicação FP32 convencional. O SGLang usa dois componentes BF16 dos pesos FP32, com acumulação FP32, transformação expressamente permitida pelas regras. A primeira variante combina esses componentes no mesmo kernel e avalia a divisão paralela da dimensão de redução nas projeções de lotes pequenos.

As ativações e os pesos das demais projeções permanecem BF16. Nenhuma variante pode armazenar resultados derivados das ativações entre chamadas, omitir contribuições ou modificar o avaliador.

O perfil da versão 1 confirmou uma aceleração de 1,43–3,41× no roteador com até 32 tokens. Porém, as projeções `o` e `unpooler` com 16/32 tokens pioraram. A versão 2 restaura o caminho original do SGLang em todas as projeções BF16 e aumenta o bloco de linhas do roteador nos lotes maiores, reduzindo releituras dos pesos.

Na versão 3, a transposição do `pooler` ganhou 5–10% nos seis lotes. Na versão 4, as projeções `qkv_gate` e `o` voltam ao caminho original, o `pooler` e o `unpooler` completo usam os pesos transpostos, e o roteador de um único token usa diretamente os pesos FP32 originais em uma chamada. Os demais lotes do roteador mantêm a divisão paralela validada na versão 2.

O check da versão 4 mediu 2,62× no roteador de um token (10,53 → 4,01 µs), 1,67–3,57× nos outros lotes do roteador, e melhorias consistentes no `pooler`. Os caminhos BF16 genéricos da versão 1 e as transposições sem ganho da versão 3 foram retirados do arquivo final. A avaliação estática oficial não apontou violações.

SHA-256 local da versão 4, publicada inicialmente: `842427d3e93311c4495ac6fbea0caf36dc34f0b51e0f1a928d24f7fd2c8d50b2`. Sua cópia está em `experimentos/publicado_v4.py`.

## Resultado inicial publicado, versão 4

A submissão pontuada confirmou o ganho em uma nova execução: **1,1635× sobre o SGLang**, com os nove testes e os trinta formatos pontuados aprovados. O intervalo de 95% ficou inteiramente acima de 1×. A pontuação normalizada foi de **4.151,86 µs**, contra **4.830,80 µs** do SGLang, uma redução de aproximadamente **14,1%**.

Na consulta de 22/10/2026, a conta `betitanx` estava provisoriamente em **64º de 97** no problema. O placar está em https://openkernelchallenge.dev/problems/muse_bf16_proj. As posições dependem da revisão final da organização e podem mudar com novas submissões.

O check final e a submissão pontuada têm o mesmo identificador de conteúdo no serviço: `c690763cd91245e0b4b21d0bf64daa6c1a95ed5fb295954173d1e2bcd2063499`. Esse identificador é registrado separadamente do SHA-256 local dos bytes do arquivo.

Foram usados cinco checks e uma submissão pontuada. A última consulta mostrou quinze checks e nove submissões pontuadas restantes na cota diária deste problema. As cotas reiniciam às 21h de Brasília, equivalentes a 00h UTC.

## Busca pelo top 5

Objetivo atualizado: continuar otimizando este problema para entrar no top 5, depois buscar o primeiro lugar e manter a posição, usando os checks disponíveis e submetendo apenas versões com ganho comprovado sobre a anterior. Na primeira consulta desta rodada, o quinto colocado tinha 1,2948×, e a versão publicada tinha 1,1635×. Portanto, faltava aproximadamente 11,3% de aceleração relativa.

O arquivo principal corresponde à versão 14, enviada ao placar após o check comprovar melhora sobre a versão 4: o limite inferior de 1,2205× supera o limite superior anterior de 1,1810×. As demais alternativas permanecem em `experimentos/`. Antes de outra submissão pontuada, a combinação escolhida deve passar nos testes e superar a versão publicada por uma margem que exceda a incerteza das medições. Resultados isolados dentro do ruído não justificam uma submissão.

A versão 14 combina GEMV para projeções de um token, blocos transpostos nas projeções `qkv_gate` e `pooler` pequenas, TinyGEMM no `unpooler` de dois a oito tokens, cuBLAS nas demais projeções e o roteador intercalado com blocos maiores acima de 192 tokens. Todas as decisões usam somente metadados dos tensores e da configuração.

Foram usados os quinze checks disponíveis no início desta rodada, totalizando vinte no dia. Todos terminaram. A versão 14 continua sendo a melhor versão publicada e validada desta rodada. Nenhuma rotina usa o autotuner da versão 13 na versão publicada: o grafo CUDA deve executar diretamente um algoritmo previamente definido ou um fallback normal da biblioteca, sem medições dentro da captura.

## Resultado atual publicado, versão 14

A avaliação pontuada `803cbbd3eee447fa` confirmou **1,2256×**, com intervalo de 95% de **1,2072–1,2440×**, nove testes e trinta formatos aprovados, sem violações apontadas. O ganho de velocidade sobre a versão 4 é de aproximadamente **5,3%**, confirmado por intervalos que não se sobrepõem.

A consulta salva em `resultados/placar-v14.json` mostrou **35º de 106**; a consulta posterior em `resultados/placar-fim-rodada.json` mostrou **36º de 111**. O quinto colocado tinha **1,2959×**, portanto faltavam aproximadamente **5,7%** de aceleração relativa para esse corte. O top 5 ainda não foi alcançado.

SHA-256 local da versão 14: `1ed5c8403f907ed81c28356966f1e527a5a8b6ae5c206b1f63965d76b868e0ea`. O arquivo principal é uma cópia exata de `experimentos/combinado_v14.py`.

O ajuste menor de blocos em `experimentos/ajuste_blocos_nao_avaliado.py` foi preparado, mas não consumiu check, não foi medido e não foi publicado.

Fontes usadas para formular hipóteses:

- [Tutorial de multiplicação matricial do Triton](https://triton-lang.org/main/getting-started/tutorials/03-matrix-multiplication.html): blocos, acumulação FP32 e agrupamento de programas para reúso em cache.
- [Tutorial de multiplicação persistente e TMA do Triton](https://triton-lang.org/main/getting-started/tutorials/09-persistent-matmul.html): descritores de tensores, transferências assíncronas e organização do trabalho no Hopper.
- [FlashDecoding++, MLSys 2024](https://proceedings.mlsys.org/paper_files/paper/2024/file/5321b1dabcd2be188d796c21b733e8c7-Paper-Conference.pdf): escolha de fluxos distintos para GEMV e matrizes com poucas linhas. A hipótese GEMV foi medida especificamente na H200 do desafio.
- [TinyGEMM BF16 do FlashInfer](https://docs.flashinfer.ai/generated/flashinfer.gemm.tinygemm_bf16.html): implementação para SM90+, BF16 com acumulação FP32, voltada à latência de lotes pequenos.
- [Especialização de warps no Triton, PyTorch](https://pytorch.org/blog/warp-specialization-in-triton-design-and-roadmap/): particionamento de transferências e cálculo. O experimento com descritores na GPU compilou corretamente, mas não foi competitivo neste conjunto de matrizes.
- [CUTLASS Ping-Pong, PyTorch](https://pytorch.org/blog/cutlass-ping-pong-gemm-kernel/): sobreposição de cópias, cálculo e epílogo com grupos de warps especializados. É uma hipótese futura, ainda não implementada nem avaliada neste projeto.
- [Stream-K](https://arxiv.org/abs/2301.03598): decomposição do trabalho para melhorar ocupação e equilíbrio de cargas em GEMM. É uma hipótese futura, ainda não implementada nem avaliada neste projeto.

## Reprodução dos experimentos

No PowerShell, a partir de `D:\Projetos DEV\open-kernel-challenge`:

```powershell
.\okc.ps1 check muse_bf16_proj/submission.py --no-wait --json
.\consultar_resultado.ps1 -Id <identificador>
```

Os relatórios completos ficam em `resultados/`, e os perfis oficiais de cada experimento ficam em `experimentos/profiles/`. O perfil da submissão pontuada está em `profiles/submission.19a230ce058a454d.txt`, com os dados completos no JSON de mesmo nome. Checks não entram no placar.

# Histórico da pesquisa

Este arquivo organiza a participação de Bernardo na vertente muse_bf16_proj. A navegação resumida acompanha os registros completos em texto e um catálogo dos resultados locais.

## Percurso

| Frente | Estratégia e aprendizado |
|---|---|
| Primeiras versões | Medir contra SGLang, identificar diferenças entre lotes e separar correção de desempenho. |
| GEMV e pesos BF16 | Vetorização, compactação sem perda, reconstrução dos pesos e correções de valores fora da faixa. |
| GEMM e redução | Geometrias, split-K, TMA, WGMMA, compartilhamento entre CTAs e custos de redução. |
| Bibliotecas | Comparar e fixar planos cuBLASLt, validar alinhamento, precisão e a execução efetiva do caminho escolhido. |
| Integração de fontes públicas | Preservar licenças e atribuições, comparar a combinação integral e corrigir ownership de estado. |
| Publicação v245 | Reprodução legível de keell, 1,574877619× na pontuada e melhoria de 3,96% sobre a publicação válida anterior. |
| Reta final | Quinze checks e uma pontuada. Candidatas corretas foram arquivadas quando o ganho não superou a incerteza. |
| Encerramento | 192ª posição provisória às 08h UTC, 194ª na consulta posterior. O objetivo top 5 não foi atingido. |

## Documentação preservada

- [Todas as avaliações oficiais](avaliacoes.md) reúne 121 checks e cinco pontuadas em ordem cronológica.
- [Dados das avaliações](avaliacoes.json) registra resultados globais, intervalos, erros e os formatos medidos de cada job.
- [Catálogo de pesquisas](pesquisas.json) preserva estados, hipóteses, decisões, limitações e hashes dos relatórios locais.
- [Catálogo das fontes](catalogo-de-fontes.json) identifica as versões de código por caminho, tamanho e SHA-256.
- [Diário completo do laboratório](laboratorio-completo.md) preserva a narrativa dos experimentos e suas revisões.
- [Estado até v138](estado-ate-v138.md), [continuidade até v128](continuidade-ate-v128.md) e [continuidade até v138](continuidade-ate-v138.md) mantêm o histórico anterior às consolidações.
- [Resumo de 04/11](resumo-04-11.md), [resumo de 05/11](resumo-05-11.md), [desempenho](desempenho.md) e [cobertura](cobertura.md) registram as sínteses finais.

As referências a caminhos do laboratório nos documentos legados identificam o arquivo original. Os catálogos preservam esses identificadores. O repositório distribui os três códigos selecionados em ../solucoes, enquanto os demais experimentos são representados pelos registros históricos e hashes.

## Resultado e limites

Foram 126 avaliações oficiais, 120 hashes de código distintos e 186 entradas nativas emitidas localmente. Há dez avaliações não integrais e uma desclassificação conhecida. A antiga publicação de 1,540954514× foi desclassificada por fonte compactada decodificada em runtime. O histórico conserva esse resultado com a decisão correspondente.

A candidata H238 obteve 1,595119240× mantendo o plano parental. H245 obteve 1,582186524× com o novo router QS8, sem melhora consistente sobre as duas referências. H239 sugeriu ganho no componente pooler32, mas a versão isolada permaneceu sem check integral. As decisões preservam a distinção entre código emitido localmente, correção aprovada e ganho medido na H200.

## Escopo do arquivo

O histórico contém os registros textuais e metadados da pesquisa. Binários compilados, caches, pacotes do SDK, o sistema WSL e cópias descartadas de fontes não fazem parte deste arquivo simplificado. As três soluções selecionadas mantêm seus bytes originais e as licenças incorporadas.
